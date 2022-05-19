# AutomatingTranslation
you can choose a document with all the words you want to translate to any language to deepL translator website. The following codes will translate all the words automatically. 
you will need to open inspection pannel. Then, you will need to delete ```<header>``` in the elements. Then, you will insert ```<input type="file" id="file-input">```into the top part of the body.
Run the following code from console. 
Choose the file that you want to translate by using the 'choose file' button. e.g 
```
"key_1"="sentence_to_translate_1";
"key_2"="sentence_to_translate_2";
"key_3"="sentence_to_translate_3";
"key_4"="sentence_to_translate_4";
```
The program will give a txt file at the end.
Note: translation could stop when you try to translate the same words twice in a row. When that happens, simply just click on x button or cancale button and it will continue translating.
```javascript
var keywords = [];
var index = 1;
var output = ""

parse = (contents) => {
    var text = contents;
    var lines = text.split(/\r?\n/)
    lines.forEach((line) => {
     words = line.split('"')
     if (words.length == 5){
        keywords.push(words[1])
        keywords.push(words[3])
        }
    })

}
function saveDynamicDataToFile() {



            var blob = new Blob([output], { type: "text/plain;charset=utf-8" });
            saveAs(blob, "dynamic.txt");
        }
readSingleFile = (e) => {
    var file = e.target.files[0];
    if (!file) {
        return;
    }
    var reader = new FileReader();
    reader.onload = (e) => {
        var contents = e.target.result;
        parse(contents);
    };
    reader.readAsText(file);
}
document.getElementById('file-input').addEventListener('change', readSingleFile, false);

var element = document.getElementsByClassName('lmt__textarea lmt__source_textarea lmt__textarea_base_style')[0]
var element_to_observe = document.getElementById('target-dummydiv');
var evt = document.createEvent('Events')
evt.initEvent('change', true, true)

var observer = new MutationObserver((mutationsList, observer) => {
    output = output + '"' + keywords[index - 1] + '"="' + element_to_observe.innerHTML.trim() + '";\n';
    index+= 2
    if (index < keywords.length) {
        translate(keywords[index])
    }
    else {
        index = 1;
        alert('finish')
        saveDynamicDataToFile()
        output = ''
        keywords = []
    }
});

observer.observe(element_to_observe, { characterData: false, childList: true, attributes: false });

translate = (word) => {
    element.value = word
    element.dispatchEvent(evt)
}
```
