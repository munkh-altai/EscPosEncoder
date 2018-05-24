# esc-pos-encoder

Based on [esc-pos-encoder](https://github.com/NielsLeenheer/EscPosEncoder), add `Simplified Chinese` encode.  
基于[esc-pos-encoder](https://github.com/NielsLeenheer/EscPosEncoder)修改，添加了 `简体中文`编码。

## Usage

```shell
    npm install @hexing/esc-pos-encoder --save
```

```js
let EscPosEncoder = require('esc-pos-encoder');

let encoder = new EscPosEncoder();

let result = encoder
    .initialize()
    .text('The quick brown fox jumps over the lazy dog')
    .newline()
    .line('我是一段中文')
    .right()
    .qrcode('https://nielsleenheer.com')
    .next()
    .encode();
```


**更多用法查看原文** [esc-pos-encoder](https://github.com/NielsLeenheer/EscPosEncoder)

### Initialize

Properly initialize the printer, which means text mode is enabled and settings like code page are set to default.

    let result = encoder
        .initialize()
        .encode()

### Codepage

Set the code page of the printer. Receipt printers don't support UTF-8 or any other unicode encoding, instead the rely on legacy code pages. 

If you specify the code page, it will send a command to the printer to enable that particular code page and from then on it will automatically encode all text string to that code page. 

If you don't specify a code page, it will assume you want to print only ASCII characters and strip out any others.

    let result = encoder
        .codepage('windows1251')
        .text('Iñtërnâtiônàlizætiøn')
        .encode()

The following code pages are supported: cp437, cp737, cp850, cp775, cp852, cp855, cp857, cp858, cp860, cp861, cp862, cp863, cp864, cp865, cp866, cp869, cp1252, iso88596, windows1250, windows1251, windows1252, windows1253, windows1254, windows1255, windows1256, windows1257, windows1258.

### Text

Print a string of text. If the text is longer than the line width of the printer, it will automatially wrap to the next line when it reaches the maximum width. That means it could wrap right in the middle of a word.

    let result = encoder
        .text('The quick brown fox jumps over the lazy dog')
        .encode()

An optional parameter turns on word wrapping. To enable this, specify the maximum length of the line.

    let result = encoder
        .text('The quick brown fox jumps over the lazy dog', 20)
        .encode()

### Newline

Move to the beginning of the next line.

    let result = encoder
        .newline()
        .encode()

### Line

Print a line of text. This is similar to the `text()` command, except it will automatically add a `newline()` command.

    let result = encoder
        .line('The is the first line')
        .line('And this is the second')
        .encode()

This would be equal to:

    let result = encoder
        .text('The is the first line')
        .newline()
        .text('And this is the second')
        .newline()
        .encode()

An optional parameter turns on word wrapping. To enable this, specify the maximum length of the line.

    let result = encoder
        .line('The quick brown fox jumps over the lazy dog', 20)
        .encode()

### Underline

Change the text style to underline. 

    let result = encoder
        .text('This is ')
        .underline()
        .text('underlined')
        .underline()
        .encode()

It will try to remember the current state of the text style. But you can also provide and additional parameter to force the text style to turn on and off.

    let result = encoder
        .text('This is ')
        .underline(true)
        .text('bold')
        .underline(false)
        .encode()

### Bold

Change the text style to bold. 

    let result = encoder
        .text('This is ')
        .bold()
        .text('underlined')
        .bold()
        .encode()

It will try to remember the current state of the text style. But you can also provide and additional parameter to force the text style to turn on and off.

    let result = encoder
        .text('This is ')
        .bold(true)
        .text('bold')
        .bold(false)
        .encode()

### Size

Change the text size. You can specify the size using a parameter which can be either "small" or "normal".

    let result = encoder
        .size('small')
        .line('A line of small text)
        .size('normal')
        .line('A line of normal text)
        .encode()

### Barcode

Print a barcode of a certain symbology. The first parameter is the value of the barcode, the second is the symbology and finally the height of the barcode.

The following symbologies can be used: 'upca', 'ean13', 'ean8', 'code39', 'itf', 'codabar'.

    let result = encoder
        .barcode('3130630574613', 'ean13', 60)
        .encode()


### Qrcode

Print a QR code. The first parameter is the value of the QR code.

    let result = encoder
        .qrcode('https://nielsleenheer.com')
        .encode()


### Image

**Not supported**

Print an image. The image is automatically converted to black and white and can optionally be dithered using different algorithms.

The first parameter is the image itself. When running in the browser it can be any element that can be drawn onto a canvas, like an img, svg, canvas and video elements. When on Node it can be a Canvas provided by the `canvas` package. 

The second parameter is the width of the image on the paper receipt in pixels. It must be a multiple of 8.

The third parameter is the height of the image on the paper receipt in pixels. It must be a multiple of 8.

The fourth parameter is the dithering algorithm that is used to turn colour and grayscale images into black and white. The follow algorithms are supported: threshold, bayer, floydsteinberg, atkinson. If not supplied, it will default to a simple threshold.

The fifth paramter is the threshold that will be used by the threshold and bayer dithering algorithm. It is ignored by the other algorithms. It is set to a default of 128.

    let img = new Image();
    img.src = 'https://...';
    
    img.onload = function() {
        let result = encoder
            .image(img, 300, 300, 'atkinson')
            .encode()
    }

## next

Set printer to the next page.

## left

Set text align left.

## center

Set text align center.

## right

Set text align right.

## License

MIT