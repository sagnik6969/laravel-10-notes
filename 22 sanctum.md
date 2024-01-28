##                                  LARAVEL SANCTUM

Introducing laravel sanctum ( sanctum >>> JWT )

Laravel sanctum is a authentication system for SPAs, mobile applications and simple `token based APIs`.

Working: Sanctum allows each user of your application to generate multiple API tokens for their account. These tokens may be granted abilities / scopes which specify which actions the tokens are allowed to perform.

read the docs: `https://laravel.com/docs/10.x/sanctum#main-content`
read article: `https://dev.to/olodocoder/laravel-api-series-laravel-sanctum-setup-sign-up-login-and-logout-3kb`


# Step 1:
uncomment the line under `api` module.

# syntex

{{
    Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
    });

}}
#                                      OR

# inside controller

{{
     public function __construct() {$this->middleware('auth:sanctum')->except(['index','show']);}
    
}}


# ----> login using username and password

{{
       if (!Hash::check($request->password, $user->password)){
            
            throw ValidationException::withMessages([
                'email' => ['The provided credentials are incorrect']
            ]);
        }

        $token = $user->createToken('api-token')->plainTextToken;
       
        return response()->json([
            'token'=>$token
        ]);
        
}}

# ----> signing out

{{
     \$request->user()->tokens()->delete();
}}

# ----> to specify token expiration

--> Inside `config/sanctum.php`
{{
    'expiration' => 60 * 24,
}}