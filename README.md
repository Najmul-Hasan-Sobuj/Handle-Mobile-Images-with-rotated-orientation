# Handle-Mobile-Images-with-rotated-orientation


Here's a step-by-step guide on how to use the Intervention Image library in Laravel to handle mobile images with rotated orientation:

Step 1: Install Intervention Image
In your Laravel project, open the terminal and run the following command to install the Intervention Image package:

```bash
composer require intervention/image
```

Step 2: Configure Service Provider
After installing the package, open the `config/app.php` file and add the following line to the `providers` array:

```php
'providers' => [
    // Other service providers...
    Intervention\Image\ImageServiceProvider::class,
],
```

Step 3: Register the Image Facade (Optional)
If you want to use the Image facade, add the following line to the `aliases` array in the same `config/app.php` file:

```php
'aliases' => [
    // Other aliases...
    'Image' => Intervention\Image\Facades\Image::class,
],
```

Step 4: Use Intervention Image in Your Controller
Next, create a controller (if you don't have one already) to handle image uploads. In this controller, import the Intervention Image class at the top:

```php
use Intervention\Image\Facades\Image;
```

Step 5: Handle Image Upload and Rotation
In your controller method responsible for handling image uploads, you can do the following:

```php
public function uploadImage(Request $request)
{
    // Validate and process the image upload as you normally would

    // Get the uploaded file
    $uploadedImage = $request->file('image');

    // Read the EXIF orientation data
    $image = Image::make($uploadedImage);
    $orientation = $image->exif('Orientation');

    // Correct the image orientation if necessary
    if ($orientation) {
        switch ($orientation) {
            case 3:
                $image->rotate(180);
                break;
            case 6:
                $image->rotate(-90);
                break;
            case 8:
                $image->rotate(90);
                break;
        }
    }

    // Save the image
    $image->save(public_path('path/to/your/uploads/directory') . '/' . $uploadedImage->getClientOriginalName());

    // Continue with the rest of your logic, if needed
}
```

Step 6: Displaying the Uploaded Image
To display the uploaded image in your views, you can use the standard HTML `<img>` tag with the URL of the uploaded image. Make sure to point it to the correct path based on the directory you specified in the `public_path` method during the image save.

And that's it! You now have a working solution to handle mobile images with rotated orientation in Laravel using the Intervention Image library.