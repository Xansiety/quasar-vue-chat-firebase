## Instalar firebase

`yarn add firebase` o `npm i firabase`

## firebase initail

Permite iniciar de una las configuraciones iniciales sin preguntar tanta cosa y pasa irecto al hosting

`firebase init hosting`

- En la opcion de carpeta que se deplegara debera apuntar a "dist/spa",
  o bien se puede configurar despues en el archivo firebase.json "public": "dist/spa",

## Deploy

Se debe compilar aplicacion con quasar dev

Y ejecutar en la termina

`firebase deploy --only hosting`
