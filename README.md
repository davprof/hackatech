# Setup formulário

## Google Forms

![image](https://github.com/user-attachments/assets/5ce170f3-c075-4818-862a-f8c08556ba5c)

## Setup Sheet

![image](https://github.com/user-attachments/assets/499618a1-90d7-41a6-96ed-cbd6600593bc)

## Setup Apps Script

![image](https://github.com/user-attachments/assets/0ca1d033-13b3-4a3a-9eca-8b1daf6fe170)

## No editor substitua o código pelo seguinte script

```js
function onFormSubmit(e) {
    const FORM_ID = '1AQY9Ad36nN-WEMSvW_FHc2o2gBhSCzQk8244v0o0soY';
    const SHEET_NAME = 'Respostas ao formulário 1';
    const MEMBERS_PER_TEAM = 5;
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
    var responses = sheet.getRange('B:B').getValues().flat().filter(x => x);
    var count = {};
    
    // Contar as seleções
    for (var i = 1; i < responses.length; i++) {
        const teamId = responses[i];
        const teamSize = count[teamId] || 0;
        if (teamId) {
            count[teamId] = teamSize + 1;
        }
    }
    
    // Atualizar a disponibilidade
    var form = FormApp.openById(FORM_ID);
    var items = form.getItems(FormApp.ItemType.MULTIPLE_CHOICE);
    var item = items[0].asMultipleChoiceItem();
    var choices = item.getChoices();
    
    for (var i = 0; i < choices.length; i++) {
        var choice = choices[i].getValue();
        if (count[choice] >= MEMBERS_PER_TEAM) {
            choices.splice(i, 1)
        }
    }
    
    if (choices.length > 0) {
      item.setChoices(choices);
    } else {
      item.setChoices([item.createChoice("Sem vagas")]);
    }
}
```

## Em acionadores adicione uma nova execução da função onFormSubmit no submit da planilha.

## Teste seu formulário
