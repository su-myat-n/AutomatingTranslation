# AutomatingTranslation
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
