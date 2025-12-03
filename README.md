
**Características:**

Algoritmo: AES (Advanced Encryption Standard)
Modo: CBC (Cipher Block Chaining) por defecto. CBC divide el mensaje en bloques y combina cada bloque con el anterior antes de cifrarlo.
Padding: PKCS#7. Rellena el último bloque si el mensaje no completa un bloque de 16 bytes.
Clave: derivada de la contraseña con EVP_BytesToKey + salt aleatorio.
Resultado: Base64 con salt incluido.

- Simétrico: La misma contraseña debe usarse para cifrar y descifrar.
- Seguro: AES es considerado seguro si la clave es suficientemente larga (mínimo 128 bits recomendados).
- Sencillo: CryptoJS maneja internamente IV (vector de inicialización) y salt. Cada cifrado del mismo mensaje genera resultados diferentes.

Modo CBC:
- Es seguro, pero requiere IV (CryptoJS lo genera internamente) y no incluye integridad.
- No detecta si el mensaje ha sido modificado (no hay autenticación).

AES-256 vs AES-128:
- CryptoJS decide automáticamente la longitud de clave según la derivación.
- Si quieres máxima seguridad, conviene usar AES-256 explícitamente.

Autenticación de mensaje:
- Actualmente no estás usando HMAC ni AES-GCM.
- CBC no protege contra manipulaciones del mensaje.
- Para seguridad máxima se recomienda AES-GCM (proporciona cifrado + autenticidad) en lugar de CBC.
- AES-GCM no está soportado por CryptoJS.

**Cómo mejorar la seguridad:**

- Usar contraseñas largas y complejas (mínimo 12 caracteres, mezcla de letras, números y símbolos).
- Usar AES con autenticación (AES-GCM) si tu implementación lo permite (CryptoJs no).
- Agregar HMAC al mensaje cifrado para detectar modificaciones.
- Usar derivación de clave fuerte, como PBKDF2 o scrypt, en lugar de EVP_BytesToKey.

**Contraseñas**:

- La seguridad depende de la fuerza de tu contraseña.
- Si la contraseña es corta o fácil de adivinar, AES no ayuda mucho.
- Usa mezcla de letras mayúsculas/minúsculas, números y símbolos: !@#$%^&*()-_=+[]{}<>?
- Evita palabras del diccionario o nombres propios.
- Considera frases de contraseña:
  - Ejemplo: MiGato$Come2Pescados! → fácil de recordar, fuerte y >16 caracteres.
- Si usas derivación moderna (PBKDF2 o scrypt), puedes usar contraseñas más cortas, pero mejor mínimo 12 caracteres.

**Notas:**

- Esta versión usa la implementación con CryptoJS: 

| Implementación             | Derivación clave | AES modo | Seguridad principal                        |
| -------------------------  | ---------------- | -------- | ------------------------------------------ |
| **Código CryptoJS actual** | EVP_BytesToKey   | CBC      | Rápido pero débil para contraseñas humanas |

**Mayor seguridad:**

| Implementación             | Derivación clave | AES modo | Seguridad principal                        |
| AES-GCM + PBKDF2           | PBKDF2           | GCM      | Muy seguro, confidencialidad + integridad  |
| AES + scrypt               | scrypt           | GCM/CBC  | Máxima seguridad frente a ataques modernos |




