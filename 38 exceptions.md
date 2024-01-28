# ----> to throw a dalidation exception

{{
    throw ValidationException::withMessages([
                'email' => ['The provided credentials are incorrect']
            ]);
}}