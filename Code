function doGet() {
  return HtmlService.createTemplateFromFile('Index').evaluate()
  .setTitle('Form Input Nota Transaksi')
  .addMetaTag('viewport', 'width=device-width, initial-scale=1')
  .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.DEFAULT)
}


/* DEFINE GLOBAL VARIABLES, CHANGE THESE VARIABLES TO MATCH WITH YOUR SHEET */
function globalVariables(){ 
  var varArray = {
    spreadsheetId   : '1Hh9cu56SLZ88vPIHzaMlgsIsmguqps669sUe1nIARo4',
    dataRage        : 'Master_RDA!A2:K',                                    
    idRange         : 'Master_RDA!A2:A',                                    
    lastCol         : 'K',                                            
    insertRange     : 'Master_RDA!A1:K1',                                   
    sheetID         : '0'                                             
  };
  return varArray;
}

/*
# PROCESSING FORM ---------------------------------------------------------------------------------
*/


/* PROCESS FORM */
function processForm(formObject){  
  if(formObject.actID && checkID(formObject.actID)){//Execute if form passes an ID and if is an existing ID
    updateData(getFormValues(formObject),globalVariables().spreadsheetId,getRangeByID(formObject.actID)); // Update Data
  }else{ //Execute if form does not pass an ID
    appendData(getFormValues(formObject),globalVariables().spreadsheetId,globalVariables().insertRange); //Append Form Data
  }
  return getAllData();//Return last 10 rows
}

var folder = DriveApp.getFolderById('1KdvC3F5Fsq0OLdmuBYjY-41gZH2P0YEI');
function getFormValues(formObject){
  if(formObject.actID && checkID(formObject.actID)){
    let file = folder.createFile(formObject.myFile).getUrl()
    var values = [[
                  formObject.actID.toString(),
                  formObject.date,
                  formObject.projectID,
                  formObject.picName,
                  formObject.detailAct,
                  formObject.cost,
                  formObject.city,
                  formObject.siteName,
                  formObject.costCA,
                  formObject.resultDet,
                  file]];
  }else{
    let file = folder.createFile(formObject.myFile).getUrl()
    var values = [[
                  formObject.date,
                  formObject.projectID,
                  formObject.picName,
                  formObject.detailAct,
                  formObject.cost,
                  formObject.city,
                  formObject.siteName,
                  formObject.costCA,
                  formObject.resultDet,
                  file]];
  }
  return values;
}

/* CREATE/ APPEND DATA */
function appendData(values, spreadsheetId,range){
  var valueRange = Sheets.newRowData();
  valueRange.values = values;
  var appendRequest = Sheets.newAppendCellsRequest();
  appendRequest.sheetID = spreadsheetId;
  appendRequest.rows = valueRange;
  var results = Sheets.Spreadsheets.Values.append(valueRange, spreadsheetId, range,{valueInputOption: "RAW"});
}


/* READ DATA */
function readData(spreadsheetId,range){
  var result = Sheets.Spreadsheets.Values.get(spreadsheetId, range);
  return result.values;
}


/* UPDATE DATA */
function updateData(values,spreadsheetId,range){
  var valueRange = Sheets.newValueRange();
  valueRange.values = values;
  var result = Sheets.Spreadsheets.Values.update(valueRange, spreadsheetId, range, {
  valueInputOption: "RAW"});
}


/* CHECK FOR EXISTING ID, RETURN BOOLEAN */
function checkID(actID){
  var idList = readData(globalVariables().spreadsheetId,globalVariables().idRange,).reduce(function(a,b){return a.concat(b);});
  return idList.includes(actID);
}


/* GET DATA RANGE A1 NOTATION FOR GIVEN ID */
function getRangeByID(id){
  if(id){
    var idList = readData(globalVariables().spreadsheetId,globalVariables().idRange);
    for(var i=0;i<idList.length;i++){
      if(id==idList[i][0]){
        return 'Master_RDA!A'+(i+2)+':'+globalVariables().lastCol+(i+2);
      }
    }
  }
}


/* GET RECORD BY ID */
function getRecordById(id){
  if(id && checkID(id)){
    var result = readData(globalVariables().spreadsheetId,getRangeByID(id));
    return result;
  }
}


/* GET ROW NUMBER FOR GIVEN ID */
function getRowIndexByID(id){
  if(id){
    var idList = readData(globalVariables().spreadsheetId,globalVariables().idRange);
    for(var i=0;i<idList.length;i++){
      if(id==idList[i][0]){
        var rowIndex = parseInt(i+1);
        return rowIndex;
      }
    }
  }
}


/* GET ALL RECORDS */
function getAllData(){
  var data = readData(globalVariables().spreadsheetId,globalVariables().dataRage);
  return data;
}


/* INCLUDE HTML PARTS, EG. JAVASCRIPT, CSS, OTHER HTML FILES */
function include(filename) {
  return HtmlService.createHtmlOutputFromFile(filename)
      .getContent();
}