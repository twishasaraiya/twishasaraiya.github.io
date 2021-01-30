Recently I was working on a feature to read attachments from inbox. Thats when I faced an issue for rendering inline images in the application. The requirement was that the attachments should be shown separate and inline images should be shown as part of the email body itself. So now that we know what is the requirement lets deep dive into it then!

### Pre-requisites 
- Angular > 7

### Problem

The images in the email body could be divided into 3 broad categories
  1. Attachment
  2. Inline Image with https url/deployed url
  3. Inline Image that was uploaded from local
  
In this post we are going to look into the third category and how to resolve it 

### Solution

Lets take an example and work on it. For a working implementation of the same you can refer [Stackblitz](https://angular-ivy-x9zazq.stackblitz.io)
Lets say, the backend APIs return data in the format given below

```
{
  ...,
  emailBody: '<html><head> </head><body> ..<img src=\"cid:XXXXXXXXX\" alt=\"react.png\" width=\"452\" height=\"237\">..... </body></html>',
  ...
}
```

Now when you try to render this emailBody body,it will definitely not show up 

```html
<div [innerHTML]="emailBody"></div>
```

In order to render this image, we need to do certain things

#### Step 1. Parse the HTML and find all `img` tags 

To find all the image tags in the html string, we use the regex pattern as follows `<img.*?src=\\"(.*?)\\"[^\>]+>`. What this regex does is identiefies all images with any value in the src property and any other properties that might exist with `src` like `width`, `height` or `style`.

```javascript
regex = new RegExp('<img.*?src=\\"(.*?)\\"[^\>]+>',"g");
let inlineImages = emailBody.match(regex);
```

The images that we need to handle here are the ones that were uploaded from computer or mobile by the user. So the API has uploaded these images on the bucket and provided an ID in the src in the format `cid:XXXXXX`. 

#### Step 2. Loop through inline images and filter those `img` tags based on `src="cid:XXXXXX"`. Why filter it out? Because we only want to replace the uploaded images from the public url images

```javascript
inlineImages = inlineImages.filter(inlineImage => src.startsWith('cid'));
```

Now that you have all the images and their, you call the APIs and get the image contents for each of those IDs that were extracted from `src`. The APIs return base64 image string in the respone. 

#### Step 3. Convert the [base64](https://developer.mozilla.org/en-US/docs/Glossary/Base64) image to a [blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)

In this step, we write a generic function to do the job of converting a base64 image to a blob.
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

#### Step 4. Create a dummy URL for the blob

```javascript
let url = URL.createObjectURL(blob);
```
To learn more about this method you can refer [this](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL)

#### Step 5. Replace `src="cid:XXXXXX"` with the dummy url

```javascript
let newImageTag = imgTag.replace(/src=".*?"/,`src="${url}"`);
emailBody = emailBody.replace(imgTag,newImageTag);
```
Even after replacing the the ID with a URL an error is thrown by angular compiler for security reasons. Next we will see on how we can resolve it.

### Step 6. Bypass the security issues 

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

### End Notes

The backend APIs that were mentioned throughout the blog are Micrsoft Graph APIs and we had an application built around it.
If you like the article and found it helpful, do let me know. Or if you have any feedback or better way of doing it, would love to hear it as well.

