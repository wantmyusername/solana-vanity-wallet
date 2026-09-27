# Solana Vanity Wallet — Tutorial

Cómo generar una **vanity wallet** de Solana: una dirección de wallet que empieza (o termina) con el texto que tú elijas, usando la CLI oficial `solana-keygen grind`.

## ¿Qué es una vanity address?

Una dirección cuya clave pública empieza o termina con una palabra o prefijo "bonito" (p. ej. `SOL...`, `COOL...`, tu nick). Se generan **probando claves aleatorias** hasta dar con una que coincida, así que el tiempo crece de forma **exponencial** con cada carácter que agregas.

---

## 1. Instalar la Solana CLI

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.18.18/install)"
```

> Revisa la versión más reciente en <https://docs.solana.com/cli/install-solana-cli-tools> y cambia `v1.18.18` si quieres.

### Añadir al PATH (macOS)

```bash
export PATH="/Users/<tu-usuario>/.local/share/solana/install/active_release/bin:$PATH"
```

Para que sea permanente, agrégalo a tu `~/.zshrc` (o `~/.bashrc`). Verifica:

```bash
solana --version
```

---

## 2. Generar la vanity wallet

```bash
solana-keygen grind --starts-with PALABRA:1
```

- `--starts-with PALABRA` → la dirección debe **empezar** con ese texto.
- `:1` → cuántas coincidencias generar (normalmente `1`).

También existe:

```bash
solana-keygen grind --ends-with PALABRA:1   # termina con el texto
```

> **Ojo con la longitud:** cada letra extra multiplica el tiempo de búsqueda. Un prefijo de 3-4 caracteres es rápido; 5-6 puede tardar mucho; 7+ suele ser inviable sin mucho cómputo y suerte.

Al terminar, verás algo como:

```
Wrote keypair to PALABRAxxxx.json
```

Ese `.json` es tu **keypair** (incluye la clave privada).

---

## 3. Usar la wallet generada

```bash
# Ver la dirección pública del keypair generado
solana-keygen pubkey PALABRAxxxx.json

# Ajustar la CLI para usarlo como clave por defecto
solana config set --keypair PALABRAxxxx.json
solana address
```

O impórtalo en tu wallet (Phantom, Solflare, etc.) desde el contenido del keypair.

---

## 4. (Opcional) Convertir el keypair a base64 con Node.js

Si necesitas el keypair como texto base64 (por ejemplo, para pegarlo en una herramienta), crea `index.js`:

```js
// Array de bytes del keypair (los 64 valores del .json)
const arr = new Uint8Array([221, 180, 74 /* ... */]);

const bin = [];
for (let i = 0; i < arr.length; i++) {
  bin.push(String.fromCharCode(arr[i]));
}
const as_text = btoa(bin.join(""));
console.log(as_text);
```

Y ejecútalo:

```bash
node index.js
```

---

## Seguridad

- **Nunca compartas** el archivo del keypair (`*.json`) ni su clave privada. Cualquiera con él controla los fondos.
- **No lo subas a git.** Añade `*.json` (o al menos tus keypairs) a `.gitignore`.
- La vanity address **no** te protege mágicamente: el prefijo es solo estético. Trátala como cualquier wallet.
- Genera la wallet en una **máquina de confianza** y, para fondos reales, considera una hardware wallet.
- Desconfía de cualquier "generador de vanity wallets" online que pida tu clave privada.

## Requisitos

- **Solana CLI** (paso 1).
- **Node.js** solo para el snippet opcional (paso 4).
