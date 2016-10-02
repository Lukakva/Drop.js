**Please read the whole readme in order to have a clear idea of what Drop.js does.**

# Installation

Download the .zip file from github (and extract it), or clone the folder using `git clone`. Import the drop.js script in your willing HTML file.

```html
<script type="text/javascript" src="path/to/drop.js"></script>
```

# Drop constructor usage
You can initialize a drop object from constructor, without calling any additional functions.

```javascript
function dropHandler(dropData, e) {
    // ...
}
var dropzone = new Drop({
    node: "#dropzone", // a selector, could be a Node instance
    ondrop: dropHandler,
});
```

# The dropData argument
The dropData argument in ondrop method is the important stuff. It contains all the files, or strings it could retrieve from the event object. It contains 2 properties, `files` and `strings`.

Files property's value is an array containing all the files that occured in the drag operation.
Items property's value is an object containing value for every mimeType that was dropped on the node. (example and further explanations below).

# Example dropObject
```javascript
dropObject = {
    files: [{
        // usable File object
        file: File,
        // if in chrome, full relative path
        path: "Folder/Subfolder/Image.png",
    }],
    items: {
        plain: {
            type: "text/plain",
            // when URL is dropped, the plain value is also present
            value: "http://example.com",
        },
        uri_list: {
            type: "text/uri-list",
            value: "http://example.com",
        },
        html: {
            // if the link was dragged from another HTML page
            // the HTML might be present too
            type: "text/html",
            value: "<a href='http://example.com'>Link</a>",
        }
    },
}
```

# Some Notices

So, if links, texts or an image from other page is dragged and dropped to your node, there are several values present in the drag operation. Usually, there always is a text/plain value which represents an URL, a string, or just the url of the image. In other scenarios the text/uri-list might be present too, and even text/html value that contains the `<img src="...">` that can be parsed if you will.

**But** those values are presented in the `DataTransferItemList` which is only supported in chrome yet via accessing the `DataTransfer.items` property. In safari and firefox, those values are not present. But `DataTransfer.getItem` method allows us to extract data from DataTransfer by mime type.

So in that case where .items is not present, the code will extract three values, text/plain, text/uri-list or the text/html values.

The keys in the `.items` property are generated from the mime type by using the second part of mime type (after the slash) and replacing `'-'` character with `'_'`, so that it can directly be accessed.