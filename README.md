## laravel-track123-integration:

## The `laravel-track123-integration` repository is designed to integrate the Track123 tracking API with a Laravel application. Here are the key capabilities:

- **Track123 API Integration**: Provides a service class to interact with the Track123 API, allowing retrieval of tracking information.
- **Environment Configuration**: Includes environment variables for configuring API URL and key.
- **Routing**: Defines routes to handle tracking information requests.
- **Controller**: Implements a controller to manage the interaction between the service and the view.
- **View**: Displays tracking information fetched from the Track123 API.

Let me know if you need more details or specific instructions.

1. **Create a new Laravel project (if you haven't already)**:
   ```bash
   composer create-project --prefer-dist laravel/laravel track123-demo
   ```

2. **Install HTTP Client**:
   ```bash
   composer require guzzlehttp/guzzle
   ```

3. **Create Track123 Service Class**:
   - `app/Services/Track123Service.php`:
     ```php
     namespace App\Services;

     use Illuminate\Support\Facades\Http;

     class Track123Service
     {
         protected $apiUrl;
         protected $apiKey;

         public function __construct()
         {
             $this->apiUrl = config('services.track123.url');
             $this->apiKey = config('services.track123.key');
         }

         public function getTrackingInfo($trackingNumber)
         {
             $response = Http::withHeaders([
                 'Authorization' => 'Bearer ' . $this->apiKey,
             ])->get("{$this->apiUrl}/track/{$trackingNumber}");

             return $response->json();
         }
     }
     ```

4. **Environment Configuration**:
   - Add to your `.env` file:
     ```env
     TRACK123_API_URL=https://api.track123.com
     TRACK123_API_KEY=your_api_key_here
     ```

   - Update `config/services.php`:
     ```php
     return [
         // Other services...
         'track123' => [
             'url' => env('TRACK123_API_URL'),
             'key' => env('TRACK123_API_KEY'),
         ],
     ];
     ```

5. **Create Demo Route and Controller**:
   - `routes/web.php`:
     ```php
     use App\Http\Controllers\Track123Controller;

     Route::get('/track/{trackingNumber}', [Track123Controller::class, 'show']);
     ```

   - `app/Http/Controllers/Track123Controller.php`:
     ```php
     namespace App\Http\Controllers;

     use App\Services\Track123Service;
     use Illuminate\Http\Request;

     class Track123Controller extends Controller
     {
         protected $track123Service;

         public function __construct(Track123Service $track123Service)
         {
             $this->track123Service = $track123Service;
         }

         public function show($trackingNumber)
         {
             $trackingInfo = $this->track123Service->getTrackingInfo($trackingNumber);

             return view('track.show', compact('trackingInfo'));
         }
     }
     ```

6. **Create Demo View**:
   - `resources/views/track/show.blade.php`:
     ```blade
     <!DOCTYPE html>
     <html>
     <head>
         <title>Track123 Demo</title>
     </head>
     <body>
         <h1>Tracking Information</h1>
         <pre>{{ print_r($trackingInfo, true) }}</pre>
     </body>
     </html>
     ```

You can now create a repository and add these files to it. Let me know if you need further assistance!

![image](https://github.com/user-attachments/assets/367ed439-4a4f-4d6a-9ff1-aba38603056e)

