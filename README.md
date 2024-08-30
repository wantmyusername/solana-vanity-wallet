Instalar Solana-CLI
> sh -c "$(curl -sSfL https://release.solana.com/v1.18.18/install)"

En mac:

> PATH="/Users/jesusrodriguez/.local/share/solana/install/active_release/bin:$PATH"

> solana --version

> solana-keygen grind --starts-with KEYWORD_AQUI:1

Generar archivo index.js y ejecutar con Nodejs en terminal

>const arr = new Uint8Array([00,00]
>);
>const bin = [];
>for (let i = 0; i < arr.length; i++) {
>  bin.push( String.fromCharCode( arr[ i ] ) );
>}
>const as_text = btoa( bin.join( "" ) );
>console.log( as_text );
