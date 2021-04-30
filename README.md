# api_mojodesk
This project aims to connect google forms to api of mojo helpdesk


function copyform() {

  var dailySheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('DepositBonus');
  var dailySheet1 = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Wager');
  var dailySheet2 = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('FundTransfer');
  var appendSheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('OSF CPS');
  

  var depositbaseLastRow = dailySheet.getLastRow();
  var wagerLastRow = dailySheet1.getLastRow();
  var fundtransferLastRow = dailySheet2.getLastRow();
  var osfLastRow = appendSheet.getLastRow();



var signups = appendSheet.getRange("A"+osfLastRow +":S" +osfLastRow).getValues();
var criteriavalue = signups[0][4];  //criteria


var osfvalue = appendSheet.getRange("B"+osfLastRow +":S" +osfLastRow).getValues();

//Logger.log(osfvalue);


//each criteria aims to copy the new data entered to different sheets
 if (criteriavalue == "Deposit Bonus (Reload & FDB)"){
 
      var targetrange = dailySheet.getRange("B"+(depositbaseLastRow +1) +":S" +(depositbaseLastRow+1));

      targetrange.setValues(osfvalue);
      depositbased();

      
 }

else
{


  if (criteriavalue == "Wagering"){
   
          var targetrange = dailySheet1.getRange("B"+(wagerLastRow +1) +":S" +(wagerLastRow+1));

      targetrange.setValues(osfvalue);
      wager();

   }
  else{
 
      var targetrange = dailySheet2.getRange("B"+(fundtransferLastRow +1) +":S" +(fundtransferLastRow+1));

      targetrange.setValues(osfvalue);
      fundtransfer();
      
  }

}

}


function depositbased() {
  var spreadsheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('DepositBonus');
  var dailySheetRange = "DepositBonus!2" + spreadsheet.getLastRow();
  var archiveLastRow = spreadsheet.getLastRow()
  
var apisheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('API');
 var last = apisheet.getLastRow();


  var signups = spreadsheet.getRange("A"+archiveLastRow +":S" +archiveLastRow).getValues();

  var refloopid = signups[0][0]; //refid
  var timestampcheck =  new Date(signups[0][2]); //timestamp
    var emailreq = signups[0][16]; //email requestor
    var emailcc = signups[0][3]; //teamemail
    var criteriavalue = signups[0][4];  //criteria
    var username = signups[0][6];  //Username
  var productwager = signups[0][5];  //wagering product
  var depositbonus = signups[0][8]; //type of depositbonus
  var depositbonusproduct = signups[0][7];  //depositbonus product
  var reloadconcern = signups[0][11];  //reloadconcern
  var fdbconcern = signups[0][9]; //fdbconcern
  var commentfdb = signups[0][10];  //reloadconcern
  var commentreload = signups[0][12]; //fdbconcern
  var fromft = signups[0][13]; //ft from
  var toft  = signups[0][14]; //ft to
  var ftamount = signups[0][15]; //amount to ft
  var ftenable = signups[0][17]; //include enable ft
  var emailinclude = signups[0][18]; //cc'ed cs

var apicall = apisheet.getRange("A1:" + "B" +last ).getValues()
//Logger.log(emailcc)


   for( var nn=1 ; nn<=last; nn++){
    var  apicall = apisheet.getRange("A"+nn +":B"+nn).getValues();
    loopapilist = apicall[0][0]  

    if (loopapilist == emailcc)
    var loopvalue =apicall[0][1] 
    
    
    }


var message1 =  'This ticket has been treated with high priority.'+ '\n' + '\n' + 'Username: '+ username + '\n' + '\n' + 'Product: ' + depositbonusproduct + '\n' + '\n' + 'Bonus: ' + depositbonus + '\n'+ '\n'  + 'Concern: ' + fdbconcern + '\n' + '\n'+
    'Additional remarks: ' + commentfdb + '\n' + '\n'+ 'A new thread will be created for our response'+ 
    '\n'+ '\n'  + 'Thank you!'

  var message2 =  'This ticket has been treated with high priority.'+ '\n' + '\n' + 'Username: '+ username + '\n' + '\n' + 'Product: ' + depositbonusproduct + '\n' + '\n' + 'Bonus: ' + depositbonus + '\n'+ '\n'  + 'Concern: ' + reloadconcern + '\n' + '\n'+
    'Additional remarks: ' + commentreload + '\n' + '\n'+'A new thread will be created for our response'+ 
    '\n'+ '\n'  + 'Thank you!'


    if (depositbonus == "First Deposit Bonus"){
  		var payload = {
    'title': "FDB Bonus Concern: " +refloopid+' - ' + username,
    //"FDB Bonus Concern: " +refloopid+' - ' + username,
    'description': message1,
    'ticket_form_id': 5222,
    'ticket_queue_id': 3333,
    'user_id': loopvalue,
    'cc':  emailinclude 
  
  };       

    }
    else
    {
      		var payload = {
    'title': "Reload Bonus Concern: " +refloopid+' - ' + username,
    //"Reload Bonus Concern: " +refloopid+' - ' + username,
    'description': message2,
    'ticket_form_id': 5222,
    'ticket_queue_id': 5222,
    'user_id': loopvalue,
    'cc': emailinclude 
  
  };  

    }

    var API_URL = 'https://app.mojohelpdesk.com/api/v2/tickets?access_key=12346789099';

var options = {
  "method":"POST",
  "content-type":"application/json",
"payload": payload,

//"muteHttpExceptions":true
     };
     

Logger.log(options);
var response = UrlFetchApp.fetch(API_URL, options);

}

function wager() {
    var spreadsheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Wager');
  var dailySheetRange = "Wager!2" + spreadsheet.getLastRow();
  var archiveLastRow = spreadsheet.getLastRow()
  
  var apisheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('API');
 var last = apisheet.getLastRow();


  var signups = spreadsheet.getRange("A"+archiveLastRow +":S" +archiveLastRow).getValues();

  var refloopid = signups[0][0]; //refid
  var timestampcheck =  new Date(signups[0][2]); //timestamp
    var emailreq = signups[0][16]; //email requestor
    var emailcc = signups[0][3]; //teamemail
    var criteriavalue = signups[0][4];  //criteria
    var username = signups[0][6];  //Username
  var productwager = signups[0][5];  //wagering product
  var depositbonus = signups[0][8]; //type of depositbonus
  var depositbonusproduct = signups[0][7];  //depositbonus product
  var reloadconcern = signups[0][11];  //reloadconcern
  var fdbconcern = signups[0][9]; //fdbconcern
  var commentfdb = signups[0][10];  //reloadconcern
  var commentreload = signups[0][12]; //fdbconcern
  var fromft = signups[0][13]; //ft from
  var toft  = signups[0][14]; //ft to
  var ftamount = signups[0][15]; //amount to ft
  var ftenable = signups[0][17]; //include enable ft
  var emailinclude = signups[0][18]; //cc'ed cs

var apicall = apisheet.getRange("A1:" + "B" +last ).getValues()
//Logger.log(emailcc)


   for( var nn=1 ; nn<=last; nn++){
    var apicall = apisheet.getRange("A"+nn +":B"+nn).getValues();
    loopapilist = apicall[0][0]  

    if (loopapilist == emailcc)
    var loopvalue =apicall[0][1] 
    
    }


  //SpreadsheetApp.getActiveSheet().getRange(("A" +archiveLastRow)).setValue("wager#00"+refloopid);

    var message1 = 'A new thread will be created for our response.'+ '\n' + '\n' + 'Username: '+ username + '\n' + '\n' + 'Product: ' + productwager + '\n'+ '\n'  + 'Thank you!'


var payload = {
    'title': "Wagering: " +refloopid+' - ' + username,
    //"Wagering Bonus Concern: " +refloopid+' - ' + username,
    'description': message1,
    'ticket_form_id': 121323,
    'ticket_queue_id': 12314,
    'user_id': loopvalue,
    'cc':  emailinclude 
  
  };  

  var API_URL = 'https://app.mojohelpdesk.com/api/v2/tickets?access_key=12345679';

var options = {
  "method":"POST",
  "content-type":"application/json",
"payload": payload,
//"muteHttpExceptions":true
     };
     

Logger.log(options);
var response = UrlFetchApp.fetch(API_URL, options);

}


function fundtransfer() {
  var spreadsheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('FundTransfer');
  var dailySheetRange = "FundTransfer!2" + spreadsheet.getLastRow();
  var archiveLastRow = spreadsheet.getLastRow()
  
var apisheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('API');
 var last = apisheet.getLastRow();


  var signups = spreadsheet.getRange("A"+archiveLastRow +":S" +archiveLastRow).getValues();

  var refloopid = signups[0][0]; //refid
  var timestampcheck =  new Date(signups[0][2]); //timestamp
    var emailreq = signups[0][16]; //email requestor
    var emailcc = signups[0][3]; //teamemail
    var criteriavalue = signups[0][4];  //criteria
    var username = signups[0][6];  //Username
  var productwager = signups[0][5];  //wagering product
  var depositbonus = signups[0][8]; //type of depositbonus
  var depositbonusproduct = signups[0][7];  //depositbonus product
  var reloadconcern = signups[0][11];  //reloadconcern
  var fdbconcern = signups[0][9]; //fdbconcern
  var commentfdb = signups[0][10];  //reloadconcern
  var commentreload = signups[0][12]; //fdbconcern
  var fromft = signups[0][13]; //ft from
  var toft  = signups[0][14]; //ft to
  var ftamount = signups[0][15]; //amount to ft
  var ftenable = signups[0][17]; //include enable ft
  var emailinclude = signups[0][18]; //cc'ed cs

var apicall = apisheet.getRange("A1:" + "B" +last ).getValues()
//Logger.log(emailcc)


   for( var nn=1 ; nn<=last; nn++){
  var apicall = apisheet.getRange("A"+nn +":B"+nn).getValues();
    loopapilist = apicall[0][0]  

    if (loopapilist == emailcc)
    var loopvalue =apicall[0][1] 
    
    
    }


  
  var message2 = 'A new thread will be created for our response. This will automatically be escalated to Online Payment once approved.'+ '\n' + '\n' + 'Username: '+ username + '\n' + '\n' + 'Amount: ' + ftamount + '\n' + '\n' + 'Product From: '+ fromft + '\n' + 'Product To: ' + toft + '\n' + '\n'  + 'Enable Fund Transfer: ' + ftenable + ' - ' + fromft + '\n'+ '\n'  + 'Thank you!'

var payload = {
    'title': "FT: " +refloopid+' - ' + username,
    //"Wagering Bonus Concern: " +refloopid+' - ' + username,
    'description': message2,
    'ticket_form_id': 1231231,
    'ticket_queue_id': 231,
    'user_id': loopvalue,
    'cc':emailinclude 
  
  }; 

var API_URL = 'https://app.mojohelpdesk.com/api/v2/tickets?access_key=1231231231';

var options = {
  "method":"POST",
  "content-type":"application/json",
"payload": payload,

//"muteHttpExceptions":true
     };
     

Logger.log(options);
var response = UrlFetchApp.fetch(API_URL, options);


}
