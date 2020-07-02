So recently I was working on a feature to read attachments from inbox. Thats when I came faced an bug for rendering inline images in the application. The requirement was that the attachments should be separate and inline images should be a part of the email body itself. Lets deep dive into this then!

### Pre-requisites 
- Angular

### Problem

The images in the email body could be divided into 3 broad categories
  1. Attachment
  2. Inline Image with https url
  3. Inline Image that was uploaded
  
In this post we are going to look into the third category and how to resolve it 

### Solution

We are going to take one example and work on it. For a working implementation of the same you can refer [Stackblitz](https://angular-ivy-x9zazq.stackblitz.io)
The APIs return data in this format, lets say

```
{
  ...,
  emailBody: '..<img src=\"cid:XXXXXXXXX\" alt=\"react.png\" width=\"452\" height=\"237\">.....',
  ...
}
```

Now when you try to render this emailBody body,it will definitely not show up 

```html
<div [innerHTML]="emailBody"></div>
```

In order to render this image, we need to do certain things

1. Parse the HTML and find all `img` tags 

Very easy step using one regex pattern `<img.*?src=\\"(.*?)\\"[^\>]+>` 
```javascript
regex = new RegExp('<img.*?src=\\"(.*?)\\"[^\>]+>',"g");
let inlineImages = emailBody.match(regex);
```

2. Loop through inline images and filter those `img` tags based on `src="cid:XXXXXX"`. Why filter it out? Because we only want to replace the uploaded images and public url images

```javascript
if(src.startsWith('cid')) { ... }
```

3. Convert the [base64](https://developer.mozilla.org/en-US/docs/Glossary/Base64) image to a [blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)
(Assumption: The APIs return base64 inline images)

```javascript
const b64toBlob = (b64Data, contentType = '', sliceSize = 512) => {

    // decode a base64 encode string
    const byteCharacters = atob(b64Data);
    const byteArrays = [];
    
    for (let offset = 0; offset < byteCharacters.length; offset += sliceSize) {
      const slice = byteCharacters.slice(offset, offset + sliceSize);

      const byteNumbers = new Array(slice.length);
      for (let i = 0; i < slice.length; i++) {
        byteNumbers[i] = slice.charCodeAt(i);
      }

      const byteArray = new Uint8Array(byteNumbers);
      byteArrays.push(byteArray);
    }

    const blob = new Blob(byteArrays, { type: contentType });
    return blob;
  }
```

[Reference](https://stackoverflow.com/questions/16245767/creating-a-blob-from-a-base64-string-in-javascript) for this piece of code

4. Create a dummy URL for the blob

```javascript
let url = URL.createObjectURL(blob);
```
To learn more about this method you can refer [this](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL)

5. Replace `src="cid:XXXXXX"` with the dummy url

```javascript
let newImageTag = imgTag.replace(/src=".*?"/,`src="${url}"`);
emailBody = emailBody.replace(imgTag,newImageTag);
```
6. Bypass the security issues 

Now in your html page, you need to sanitize the url as follows
```html
<div [innerHTML]='sanitizer.bypassSecurityTrustHtml(emailBody)'></div>
```

and in your typescript file, you will need to define sanitizer
```javascript
import { DomSanitizer } from '@angular/platform-browser'
....
constructor(private sanitizer: DomSanitizer){ ... }
```

You can read more about [DOM Sanitizer](https://angular.io/api/platform-browser/DomSanitizer) to understand why we need to do this

Yayy!!!, we have now successfully rendered inline images and my attachments feature is complete!!!

### End

If like the article, do let me know. Or if you have any feedback or better way of doing it, would love to hear it.

