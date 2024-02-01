# ----> to throw a validation exception

{{
    throw ValidationException::withMessages([
                'email' => ['The provided credentials are incorrect']
            ]);
}}