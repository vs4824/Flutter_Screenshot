# Flutter Screenshot

A simple package to capture widgets as Images. Now you can also capture the widgets that are not rendered on the screen!

This package wraps your widgets inside RenderRepaintBoundary

## Getting Started 

This handy package can be used to capture any Widget including full screen screenshots & individual widgets like Text().

1. Create Instance of Screenshot Controller

   ```
   class _MyHomePageState extends State<MyHomePage> {
   int _counter = 0;
   Uint8List _imageFile;

   //Create an instance of ScreenshotController
   ScreenshotController screenshotController = ScreenshotController();

   @override
   void initState() {
   // TODO: implement initState
   super.initState();
   }
   ...
   }
   ```
   
2. Wrap the widget that you want to capture inside Screenshot Widget. Assign the controller to screenshotController that you have created earlier

   ```
   Screenshot(
    controller: screenshotController,
    child: Text("This text will be captured as image"),
   ),
   ```
   
3. Take the screenshot by calling capture method. This will return a Uint8List

   ```
   screenshotController.capture().then((Uint8List image) {
    //Capture Done
    setState(() {
        _imageFile = image;
    });
   }).catchError((onError) {
   print(onError);
   });
   ```
   
## Capturing Widgets that are not in the widget tree

You can capture invisible widgets by calling captureFromWidget and passing a widget tree to the function

   ```
   screenshotController
      .captureFromWidget(Container(
          padding: const EdgeInsets.all(30.0),
          decoration: BoxDecoration(
            border:
                Border.all(color: Colors.blueAccent, width: 5.0),
            color: Colors.redAccent,
          ),
          child: Text("This is an invisible widget")))
      .then((capturedImage) {
    // Handle captured image
  });
},
   ```

## Saving images to Specific Location

For this you can use captureAndSave method by passing directory location. By default, the captured image will be saved to Application Directory. Custom paths can be set using path parameter. Refer path_provider

   ```
   final directory = (await getApplicationDocumentsDirectory ()).path; //from path_provide package
String fileName = DateTime.now().microsecondsSinceEpoch;
path = '$directory';

screenshotController.captureAndSave(
    path //set path where screenshot will be saved
    fileName:fileName 
);
   ```

## Saving images to Gallery 

If you want to save captured image to Gallery, Please use https://github.com/hui-z/image_gallery_saver Example app uses the same to save screenshots to gallery.

   ```
   await _screenshotController.capture(delay: const Duration(milliseconds: 10)).then((Uint8List image) async {
      if (image != null) {
        final directory = await getApplicationDocumentsDirectory();
        final imagePath = await File('${directory.path}/image.png').create();
        await imagePath.writeAsBytes(image);

        /// Share Plugin
        await Share.shareFiles([imagePath.path]);
      }
    });
   ```