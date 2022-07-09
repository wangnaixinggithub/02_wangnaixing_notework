# JS-DOM-MouseEvent-Contrl-Move

- 利用JS的`MouseEvent`对象，得到当前鼠标移动的X坐标，Y坐标。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>

    </style>
    <style>
        img {
            position: absolute;
            top: 2px;
        }
    </style>
</head>

<body>
<img src="images/angel.gif" alt="">

<script>

    let imgElement = document.querySelector('img')

    document.addEventListener('mousemove',(e) =>{
        
        imgElement.style.left = e.pageX - 50 + 'px';
        imgElement.style.top = e.pageY - 40 + 'px';

    })
    
</script>
</body>

</html>
```

```js
MouseEvent {isTrusted: true, screenX: 1027, screenY: 558, clientX: 933, clientY: 414, …}
isTrusted: true
altKey: false
bubbles: true
button: 0
buttons: 0
cancelBubble: false
cancelable: true
clientX: 933
clientY: 414
composed: true
ctrlKey: false
currentTarget: null
defaultPrevented: false
detail: 0
eventPhase: 0
fromElement: null
layerX: 933
layerY: 414
metaKey: false
movementX: 51
movementY: 32
offsetX: 934
offsetY: 414
pageX: 933
pageY: 414
path: (3) [html, document, Window]
relatedTarget: null
returnValue: true
screenX: 1027
screenY: 558
shiftKey: false
sourceCapabilities: InputDeviceCapabilities {firesTouchEvents: false}
srcElement: html
target: html
timeStamp: 14959.5
toElement: html
type: "mousemove"
view: Window {window: Window, self: Window, document: document, name: '', location: Location, …}
which: 0
x: 933
y: 414
[[Prototype]]: MouseEvent
```

