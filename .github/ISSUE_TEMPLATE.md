Thanks for stopping by to let us know something could be better!

Please run down the following list and make sure you've tried the usual "quick
fixes":

  - Search the issues already opened: https://github.com/googleapis/nodejs-storage/issues
  - Search the issues on our "catch-all" repository: https://github.com/GoogleCloudPlatform/google-cloud-node
  - Search StackOverflow: http://stackoverflow.com/questions/tagged/google-cloud-platform+node.js

If you are still having issues, please be sure to include as much information as
possible:

#### Environment details

  - OS: Trying on windows & professional
  - Node.js version:8.11.1
  - npm version:5.6.0
  - `@google-cloud/storage` version:

#### Steps to reproduce

 My goal is ..
 
 I  have an android app in which i want to download an image and video,post download i am having some different logic to play with thoose files.
 
 After much r&d came to know google cloud is teh best fit, So below is the function code in which signed url will be triggering to teh database when i upload the  image, and currently because of this problem i am not able to donwload the file from the sighned url like, which makes my project haulted very badly.
 
 I need the fix immediately, without this fix my business logic will ge effected very badly,Kindly respond as soon as possible and let me know if any information required.
 
 
 Below is my code..
 
const functions = require('firebase-functions');
const gcs = require('@google-cloud/storage')({keyFilename:'xxxxx.json'})
const spawn = require('child-process-promise').spawn
const admin = require('firebase-admin')
admin.initializeApp(functions.config().firebase)
exports.generateThumbnail = functions.storage.object()
 .onChange(event => {
  const object = event.data
  const filePath = object.name
  const fileName = filePath.split('/').pop()

  const fileBucket = object.bucket

  const bucket = gcs.bucket(fileBucket)

  const tmpFilePath = '/tmp/${fileName}'

 

 

   const ref = admin.database().ref()

   const file = bucket.file(filePath)

   const thumbFilePath = filePath.replace(/(\/)?([^\/]*)$/,'$1thumb_$2')

 

   if(fileName.startsWith('thumb_')){

     console.log("already a thumb")

     return

      }

 if(!object.contentType.startsWith('image/')){

   console.log("thsi sis not an image")

    return

     }

  if(object.resouceState == 'not_exists'){

 console.log("thsi is a deletion event ")

  return

 }

 return bucket.file(filePath).download ({

 destination:tmpFilePath

   })

 
then (() => {

 console.log("file downloaded to thmp path",tmpFilePath)

return spawn('convert',[tmpFilePath,'-thumbnail','300x300>',tmpFilePath])
           })

           .then (() => {

       console.log("thumbail created")

      //const thumbFilePath = filePath.replace(/(\/)?([^\/]*)$/,'$1thumb_$2')

  return bucket.upload(tmpFilePath,{

                destination:thumbFilePath

              })

 
 }).then(() => {

 
const thumbFile = bucket.file(thumbFilePath)

const config = { action : 'write',

                   expires : '03-09-2400'

                  
}

                 return Promise.all([

                   thumbFile.getSignedUrl(config),

                   file.getSignedUrl(config)

 
 ])

 
 }).then(results => {

                  const thumbResult = results[0]

                  const originalResult = results[1]

                  const thumbFileUrl = thumbResult[0]

                  const fileUrl = originalResult[0]

 

     return ref.child('posts').push({path:fileUrl, thumbnail:thumbFileUrl})

  })

 

  })
  
Following these steps will guarantee the quickest resolution possible.

Thanks!
