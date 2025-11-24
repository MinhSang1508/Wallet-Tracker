# Asset by payment address

###

## A. BASIC CASE (not including latest transaction metadata)&#x20;

| Step | Action                   |
| ---- | ------------------------ |
| 1    | Fetch current ADA        |
| 2    | Save to Google Sheet     |
| 3    | Compare with previous    |
| 4    | Notify if changed        |
| 5    | Fetch stake + token info |
| 6    | Notify with full report  |

### 1️⃣ `ADAbalance()`

#### **Purpose**

Fetch the wallet ADA balance from Koios API and write it to Google Sheets for tracking.

#### **Key Actions**

* Define Koios API endpoint and headers
* POST request to get wallet balance
* Convert lovelace → ADA
* Append latest balance to spreadsheet

#### **Code**

```javascript
function ADAbalance() {
  var koiosUrl = "https://api.koios.rest/api/v1address_info";
  var koiosHeaders = {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "authorization": "Author token" 
  };

  var koiosBody = {
    "_payment_addresses": ["your wallet address"]
  };

  var koiosResponse = UrlFetchApp.fetch(koiosUrl, {
    method: "post",
    contentType: "application/json",
    headers: koiosHeaders,
    payload: JSON.stringify(koiosBody)
  });

  var responseData = JSON.parse(koiosResponse.getContentText());
  var balanceInfo = responseData[0];
  totalBalance = Math.round(parseFloat(balanceInfo.balance) * 1e-6);

  appendDataToSheet(totalBalance);
}
```

***

### 2️⃣ `appendDataToSheet(totalBalance)`

#### **Purpose**

Store timestamp + balance into sheet (`Summary` tab).

#### **Key Actions**

* Open spreadsheet by ID
* Append new row: `Date | Balance`

#### **Code**

```javascript
function appendDataToSheet(totalBalance) {
  var spreadsheet = SpreadsheetApp.openById(spreadsheetId);
  var sheet = spreadsheet.getSheetByName("Summary");
  var data = [new Date(), totalBalance];
  sheet.appendRow(data);
}
```

***

### 3️⃣ `getDataFromSheet()`

#### **Purpose**

Retrieve previous recorded balances to compare with the latest one.

#### **Key Actions**

* Access sheet
* Return column of balances
* Handle empty sheet safely

#### **Code**

```javascript
function getDataFromSheet() {
  try {
    var spreadsheet = SpreadsheetApp.openById(spreadsheetId);
    var sheet = spreadsheet.getSheetByName("sheetname that you created in Google Sheets");

    if (!sheet) return null;
    if (sheet.getLastRow() < 2) return null;

    var data = sheet.getRange(1, 2, sheet.getLastRow(), 1).getValues();
    return data;

  } catch (error) {
    return null;
  }
}
```

***

### 4️⃣ `compareAndPrintResults()`

#### **Purpose**

Compare latest balance vs. previous balance → detect ADA in/out.\
Send notification if changed.

#### **Key Actions**

* Detect delta value
* Write difference into column C
* Notify via Telegram + Email

#### **Code**

```javascript
function compareAndPrintResults() {

  var sheetName = "sheetname";
  var check = "https://adastat.net/accounts/...";
  var sheet = SpreadsheetApp.openById(spreadsheetId).getSheetByName(sheetName);
  var data = sheet.getDataRange().getValues();

  var message = "";
  var latestRow = data.length;
  var currentValue = data[latestRow - 1][1];

  if (latestRow > 1) {
    var previousValue = data[latestRow - 2][1];
    var result = currentValue - previousValue;
    sheet.getRange("C" + latestRow).setValue(result);

    if (result !== 0) {
      message += `<b>Your wallet has ${result > 0 ? "increased" : "decreased"} by ${result} ADA</b>\n`;
      message += `<b>Current total balance: ${currentValue} ADA</b>\n`;
      message += `Check here: ${check}\n`;
      message += `Checked at: ${currentTime}\n`;

      sendTelegramMessageToGroup(telegramBotToken, telegramChatId, message);
    }
  }

  if (message.trim()) {
    MailApp.sendEmail({
      to: recipientEmail,
      subject: "Your wallet info",
      htmlBody: message
    });
  }
}
```

***

### 5️⃣ `sendTelegramMessageToGroup()`

#### **Purpose**

Send notification to Telegram group via bot.

#### **Key Actions**

* Call Telegram Bot API
* Support HTML formatting

#### **Code**

```javascript
function sendTelegramMessageToGroup(telegramBotToken, telegramChatId, message) {
  if (message.trim() !== "") {
    var telegramApiUrl = "https://api.telegram.org/bot" + telegramBotToken + "/sendMessage";
    var payload = {
      "chat_id": telegramChatId,
      "text": message,
      "parse_mode": "HTML"
    };

    UrlFetchApp.fetch(telegramApiUrl, {
      "method": "post",
      "contentType": "application/json",
      "payload": JSON.stringify(payload)
    });
  }
}
```

***

### 6️⃣ `account_info()`

#### **Purpose**

Get full wallet info via stake key:

* Total ADA staked
* All native assets held
* Token decimals conversion
* Report via Telegram + Email

#### **Code**

```javascript
function account_info() {
var url1 = 'https://api.koios.rest/api/v1/account_info';
var url2 = 'https://api.koios.rest/api/v1/account_assets';

var payload = { '_stake_addresses': ["your stake key"] };
var options = { method: 'post', contentType: 'application/json', payload: JSON.stringify(payload) };

var data1 = JSON.parse(UrlFetchApp.fetch(url1, options).getContentText())[0];
var data2 = JSON.parse(UrlFetchApp.fetch(url2, options).getContentText());

var adabal = Math.round(parseFloat(data1.total_balance) * 1e-6);

var asset_info = [];
for (var i = 0; i < data2.length; i++) {
  var asciiAssetName = convertHexToASCII(data2[i].asset_name);
  asset_info.push([asciiAssetName, data2[i].quantity, data2[i].decimals]);
}

var sheet1 = SpreadsheetApp.openById(spreadsheetId).getSheetByName("live");
sheet1.clear();
sheet1.getRange(1, 1).setValue("ADA");
sheet1.getRange(2, 1).setValue(adabal);

// write assets
```

_(… unchanged remainder where you write to sheet + send message)_

***

### 7️⃣ `convertHexToASCII()`

#### **Purpose**

Convert Cardano token hex names → readable token name.

```javascript
function convertHexToASCII(hexString) {
  try {
    var bytes = [];
    for (var i = 0; i < hexString.length; i += 2) bytes.push(parseInt(hexString.substr(i, 2), 16));
    var blob = Utilities.newBlob(bytes);
    return blob.getDataAsString();
  } catch {
    return null;
  }
}
```

***

### 8️⃣ `getCurrentTime()`

#### **Purpose**

Return timestamp formatted as `HH:mm:ss DD/MM/YYYY`

```javascript
function getCurrentTime() {
  var d = new Date();
  return `${d.getHours()}:${d.getMinutes()}:${d.getSeconds()} ${d.getDate()}/${d.getMonth()+1}/${d.getFullYear()}`;
}
```

***

### ✅ Full code



```javascript
var spreadsheetId = "your sheet id";
var lastUpdateTimestamp = 0;
var totalBalance ;
var recipientEmail = "your email"
var check = "https://adastat.net/addresses/....." // lấy đường link trong web để copy vào đây.
var currentTime = getCurrentTime();
var telegramBotToken = " Your telegram Bot token"; // User can also store it in vari enviroment
var telegramChatId ="ID telegram group"; //add group với bot 

function ADAbalance() {
  // Define the recipient email
  var koiosUrl = "https://api.koios.rest/api/v1/address_info";
  var koiosHeaders = {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "authorization": "Author token" 
  };

  // Define the Koios API request body
  var koiosBody = {
    "_payment_addresses": ["your wallet address"]
  };

  // Make the HTTP request to Koios API
  var koiosResponse = UrlFetchApp.fetch(koiosUrl, {
    method: "post",
    contentType: "application/json",
    headers: koiosHeaders,
    payload: JSON.stringify(koiosBody)
  });

  // Parse the JSON response from Koios API
  var responseData = JSON.parse(koiosResponse.getContentText());
  var balanceInfo = responseData[0];

  // Extract relevant information from Koios API response
  totalBalance = Math.round(parseFloat(balanceInfo.balance) * 1e-6);
  var emoji = "\uD83D\uDE04"; // Emoticon '😄'

    // Add data to Google Sheet
    appendDataToSheet(totalBalance);
}

function appendDataToSheet(totalBalance) {
  // Open spreadsheet by ID
  var spreadsheet = SpreadsheetApp.openById(spreadsheetId);

  // Select sheet
  var sheet = spreadsheet.getSheetByName("Summary");

  // Append data: date + total balance
  var data = [new Date(), totalBalance];

  sheet.appendRow(data);

  console.log("Data appended to the sheet:", data);
}

function getDataFromSheet() {
  try {
    var spreadsheet = SpreadsheetApp.openById(spreadsheetId);
    var sheet = spreadsheet.getSheetByName("sheetname that you created in Google Sheets");

    if (!sheet) {
      console.error("'sheetname' not found in the spreadsheet.");
      return null;
    }

    var lastRow = sheet.getLastRow();
    if (lastRow < 2) {
      console.error("No data found in the sheet.");
      return null;
    }

    var data = sheet.getRange(1, 2, sheet.getLastRow(), 1).getValues();
    return data;
  } catch (error) {
    console.error("Error accessing the spreadsheet:", error);
    return null;
  }
}

function compareAndPrintResults() {

  var sheetName = "sheetname";
  var check = "https://adastat.net/accounts/..."
  var sheet = SpreadsheetApp.openById(spreadsheetId).getSheetByName(sheetName);
  var data = sheet.getDataRange().getValues();

  var message = "";

  var latestRow = data.length;
  var currentValue = data[latestRow - 1][1];

  if (latestRow > 1) {
    var previousValue = data[latestRow - 2][1];
    var result = currentValue - previousValue;

    sheet.getRange("C" + latestRow).setValue(result);

    if (result > 0) {
      message += "\n";
      message += "<b>Your wallet has increased by " + result + " ADA</b>\n";
      message += "\n";
      message += "<b>Current total balance: " + currentValue + " ADA</b>\n";
      message += "Check your transactions here: " + check + "\n";
      message += "Actual check time: " + currentTime + "\n";

      sendTelegramMessageToGroup(telegramBotToken, telegramChatId, message);

    } else if (result < 0) {
      message += "\n";
      message += "<b>Your wallet has decreased by " + result + " ADA</b>\n";
      message += "\n";
      message += "<b>Current total balance: " + currentValue + " ADA</b>\n";
      message += "Check your transactions here: " + check + "\n";
      message += "Actual check time: " + currentTime + "\n";

      sendTelegramMessageToGroup(telegramBotToken, telegramChatId, message);
    }
  }
  var emailSubject = "Your wallet info";
  if (message.trim() !== "") {
    MailApp.sendEmail({
      to: recipientEmail,
      subject: emailSubject,
      htmlBody: message
    });

    console.log("Comparison completed:\n" + message);
  } else {
    console.log("Message is empty. Not sending the email.");
  }
}

function sendTelegramMessageToGroup(telegramBotToken, telegramChatId, message) {
  if (message.trim() !== "") {
    var telegramApiUrl = "https://api.telegram.org/bot" + telegramBotToken + "/sendMessage";

    var payload = {
      "chat_id": telegramChatId,
      "text": message,
      "parse_mode": "HTML"
    };

    var response = UrlFetchApp.fetch(telegramApiUrl, {
      "method": "post",
      "contentType": "application/json",
      "payload": JSON.stringify(payload)
    });

    console.log(response.getContentText());
  } else {
    console.log("Message is empty. Not sending to Telegram.");
  }
}

// Get data from sheet
var sheetData = getDataFromSheet();

// Compare and send notifications if changed
compareAndPrintResults(sheetData);

function account_info() {

var url1 = 'https://api.koios.rest/api/v1/account_info';
var url2 = 'https://api.koios.rest/api/v1/account_assets';

var headers = {
  'Accept': 'application/json',
  'Content-Type': 'application/json'
};

var payload = {
  '_stake_addresses': ["your stake key"]
};

var options = {
  'method': 'post',
  'headers': headers,
  'payload': JSON.stringify(payload)
};

var responseurl1 = UrlFetchApp.fetch(url1, options);
var responseurl2 = UrlFetchApp.fetch(url2, options);

var data1 = JSON.parse(responseurl1.getContentText())[0];
var data2 = JSON.parse(responseurl2.getContentText());

var adabal = Math.round(parseFloat(data1.total_balance) * 1e-6);
var asset_info = [];
for (var i = 0; i < data2.length; i++) {
  var assetName = data2[i].asset_name;
  var quantity = data2[i].quantity;
  var decimal = data2[i].decimals;

  var asciiAssetName = convertHexToASCII(assetName);
  asset_info.push([asciiAssetName, quantity, decimal]);
}

var spreadsheet1 = SpreadsheetApp.openById(spreadsheetId);
var sheet1 = spreadsheet1.getSheetByName("live");
sheet1.clear();
sheet1.getRange(1, 1).setValue("ADA");
sheet1.getRange(2, 1).setValue(adabal);

if (asset_info.length > 0) {
  for (var i = 0; i < asset_info.length; i++) {
    sheet1.getRange(1, i + 2).setValue(asset_info[i][0]);
  }
  for (var i = 0; i < asset_info.length; i++) {
    sheet1.getRange(2, i + 2).setValue(asset_info[i][1]);
  }
  for (var i = 0; i < asset_info.length; i++) {
    sheet1.getRange(3, i + 2).setValue(asset_info[i][2]);
  }
  Logger.log(adabal);
} else {
  Logger.log("No asset_info data to write.");
}
var dataRange = sheet1.getDataRange();
var values = dataRange.getValues();

if (values.length > 1) {
  var message = "<b>Your wallet account information</b>\n\n";
  var assetNameColumn = values[0];
  var quantityColumn = values[1];
  var decimalColumn = values[2];
  var message1 ="";

  for (var i = 0; i < assetNameColumn.length; i++) {
    var assetName = assetNameColumn[i];
    var quantity = quantityColumn[i];
    var decimal = decimalColumn[i]
    if (decimal > 0) {
        quantity = quantity * Math.pow(10, -decimal);
      }

    message1 += `-  ${assetName} : <b> ${quantity} </b>\n`;
  }
    message1 += `\nActual check time: ` + currentTime + '\n';
    message1 += `Thank you for supporting VIET pool\n `;
    
  
  var finalmess = message + message1;

  sendTelegramMessageToGroup(telegramBotToken, telegramChatId, finalmess);
  var emailSubject = "Your wallet info";
  if (message.trim() !== "") {
  MailApp.sendEmail({
    to: recipientEmail,
    subject: emailSubject,
    htmlBody: finalmess
  });
  }
}else {
  send("No wallet information available.", telegramChatId);
}

}

function convertHexToASCII(hexString) {
  try {
    var bytes = [];
    for (var i = 0; i < hexString.length; i += 2) {
      bytes.push(parseInt(hexString.substr(i, 2), 16));
    }
    var blob = Utilities.newBlob(bytes);
    var asciiString = blob.getDataAsString();

    Logger.log("Converted ASCII String: " + asciiString);
    return asciiString;
  } catch (error) {
    Logger.log("Error converting hex to ASCII: " + error);
    return null;
  }
}

function getCurrentTime() {
  var currentTime = new Date();
  var hours = currentTime.getHours();
  var minutes = currentTime.getMinutes();
  var seconds = currentTime.getSeconds();
  var day = currentTime.getDate();
  var month = currentTime.getMonth() + 1;
  var year = currentTime.getFullYear();

  if (hours < 10) hours = '0' + hours;
  if (minutes < 10) minutes = '0' + minutes;
  if (seconds < 10) seconds = '0' + seconds;
  if (day < 10) day = '0' + day;
  if (month < 10) month = '0' + month;

  return hours + ':' + minutes + ':' + seconds + ' ' + day + '/' + month + '/' + year;
}

function appendDataToSheet(totalBalance) {
  var spreadsheet = SpreadsheetApp.openById(spreadsheetId);
  var sheet = spreadsheet.getSheetByName("Summary");
  var data = [new Date(), totalBalance];
  sheet.appendRow(data);
  console.log("Data appended to the sheet:", data);
}

```



## B. ADVANCE CASE ( includes latest transaction metadata)



This is an extended case based on the basic case. The user should imagine that every transaction would include a text message from the  sender (e.g, debt monthly payment, etc...)\
\
In this case, we have to track address transaction details (metadata) and record them in the sheet. However, each address would have many transactions, So we need to design a trigger that only records the data of the transaction with sequence number 1 from all the data returned from the on-chain.

#### **Code** &#x20;

```javascript
//call and fetch address transaction hash
function callKoiosAPIAndAppendTxHash(sheet, latestRow, message) {
  var koiosUrl = "https://api.koios.rest/api/v1/address_txs";
  var koiosBody = {
    "_addresses": ["put your address in here"],
    "_after_block_height": 40356
  };

  var response = UrlFetchApp.fetch(koiosUrl, {
    method: "post",
    contentType: "application/json",
    payload: JSON.stringify(koiosBody)
  });

  var responseData = JSON.parse(response.getContentText());
  if (responseData.length === 0) {
    Logger.log("No tx found.");
    return;
  }

  var txHash = responseData[0].tx_hash;
  sheet.getRange("D" + latestRow).setValue(txHash);
  Logger.log("Tx_hash: " + txHash);

  callTxInfoAPI(sheet, latestRow, txHash, message);
}

function callTxInfoAPI(sheet, latestRow, txHash, message) {
  // ---- call and fetch transaction details ----
  var infoUrl = "https://api.koios.rest/api/v1/tx_info";
  var infoBody = { "_tx_hashes": [txHash] };

  var infoRes = UrlFetchApp.fetch(infoUrl, {
    method: "post",
    contentType: "application/json",
    payload: JSON.stringify(infoBody)
  });

  var infoData = JSON.parse(infoRes.getContentText());
  if (infoData.length === 0) {
    Logger.log("No tx_info found.");
    return;
  }

  var tx = infoData[0];
  // the sender info. It is named "outputs" in the Koios database.
  var senderstakeaddress = tx.outputs[0].stake_addr || "";
  var senderaddress = tx.outputs[0].payment_addr.bech32 || "";
  sheet.getRange("F" + latestRow).setValue(senderstakeaddress);
  sheet.getRange("G" + latestRow).setValue(senderaddress);

  var hash_url = "https://api.cardanoscan.io/transaction/" + txHash;

  // ---- Gọi tx_metadata ----
  var metaUrl = "https://preview.koios.rest/api/v1/tx_metadata";
  var metaBody = { "_tx_hashes": [txHash] };

  var metaRes = UrlFetchApp.fetch(metaUrl, {
    method: "post",
    contentType: "application/json",
    payload: JSON.stringify(metaBody)
  });

var metaData = JSON.parse(metaRes.getContentText());
var msgValue = "";

if (metaData.length > 0 && metaData[0].metadata && metaData[0].metadata["674"]) {
  var meta674 = metaData[0].metadata["674"];
  if (meta674.msg && meta674.msg.length > 0) {
    msgValue = meta674.msg[0];
    sheet.getRange("E" + latestRow).setValue(msgValue);
  }
}


  // ---- Build message ----
  message += "tx hash: <a href='" + hash_url + "'>hash</a>\n";
  message += "Sender's stake address: <b>" + senderstakeaddress + "</b>\n";
  message += "Sender's payment address: <b>" + senderaddress + "</b>\n";
  if (msgValue) {
    message += "Tin nhắn của cụ: <b>" + msgValue + "</b>\n";
  }

  // ---- Send report to email + telegram ----
  MailApp.sendEmail({
    to: recipientEmail,
    subject: "Thông tin sếp",
    htmlBody: message
  });

  sendTelegramMessageToGroup(telegramBotToken, telegramChatId, message);
}

```

