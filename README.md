# Despliegue en Google Cloud Platform de una aplicación React con Nginx

Ejemplo de despliegue de una aplicación Node en Google Cloud Platform.

```bash
# Start develop
npm run start

# Build production
npm run build
```

El despliegue se realiza a través de contenedores docker.

## Prerequisitos

Tener instalado el SDK de google.

https://console.cloud.google.com/home/dashboard

## 1. Crea un projecto (example-frontend) desde la consola de Google para obtener el FRONTEND-PROJECT-ID

https://console.cloud.google.com/home/dashboard

## 2. Configuración inicial del proyecto con gcloud

Desde la raiz del proyecto ejecutar ejecuta el siguiente comando para establecer la configuración con gcloud.

```bash
gcloud init
```

```bash
➜  deploy-react-nginx git:(master) ✗ gcloud init

    Welcome! This command will take you through the configuration of gcloud.

    Pick configuration to use:
    [1] Re-initialize this configuration [app-example] with new settings
    [2] Create a new configuration
    [3] Switch to and re-initialize existing configuration: [default]
    Please enter your numeric choice:  2

    Enter configuration name. Names start with a lower case letter and
    contain only lower case letters a-z, digits 0-9, and hyphens '-':  example-frontend-config
    Your current configuration has been set to: [example-frontend-config]

    You can skip diagnostics next time by using the following flag:
      gcloud init --skip-diagnostics
```

Selecciona la cuenta desde la que se creo el proyecto.

Selecciona el proyecto creado anteriormente.

## 3. Crea la imagen

Desde la raiz del proyecto frontend ejecutar el siguiente comando:

```bash
gcloud builds submit --tag gcr.io/FRONTEND-PROJECT-ID/example-frontend
```

```bash
➜  deploy-react-nginx git:(master) ✗ gcloud builds submit --tag gcr.io/example-frontend/example-frontend

    Creating temporary tarball archive of 22 file(s) totalling 589.4 KiB before compression.
    Some files were not included in the source upload.

    Check the gcloud log [/home/alex/.config/gcloud/logs/2020.08.29/21.41.19.586147.log] to see which files and the contents of the
    default gcloudignore file used (see `$ gcloud topic gcloudignore` to learn
    more).

    Uploading tarball of [.] to [gs://example-frontend_cloudbuild/source/1598751679.610478-757513cc4b1e40f4a38daa601f9231d9.tgz]
    Created [https://cloudbuild.googleapis.com/v1/projects/example-frontend/builds/5a4db71a-48f0-47c5-956f-4bfee8b51727].
    Logs are available at [https://console.cloud.google.com/cloud-build/builds/5a4db71a-48f0-47c5-956f-4bfee8b51727?project=8836014336].
    ...
```

## 4. Crea el contenedor

```bash
gcloud run deploy --image gcr.io/FRONTEND-PROJECT-ID/example-frontend --platform managed
```

- Se te solicitará el nombre del servicio; presiona Intro para aceptar el nombre predeterminado.
- Se te solicitará la región; selecciona la región que prefieras.
- Se te solicitará permitir invocaciones no autenticadas; responde y.

```bash
➜  deploy-react-nginx git:(master) ✗ gcloud run deploy --image gcr.io/example-frontend/example-frontend --platform managed

    Service name (example-frontend):
    API [run.googleapis.com] not enabled on project [8836014336]. Would
    you like to enable and retry (this will take a few minutes)? (y/N)?  y

    Enabling service [run.googleapis.com] on project [8836014336]...
    Operation "operations/acf.1c3eef70-0a5f-461c-acac-e9f3deb0b878" finished successfully.
    Please specify a region:
    [1] asia-east1
    [2] asia-northeast1
    [3] asia-northeast2
    [4] asia-southeast1
    [5] australia-southeast1
    [6] europe-north1
    [7] europe-west1
    [8] europe-west4
    [9] northamerica-northeast1
    [10] us-central1
    [11] us-east1
    [12] us-east4
    [13] us-west1
    [14] cancel
    Please enter your numeric choice:  13

    To make this the default region, run `gcloud config set run/region us-west1`.

    Allow unauthenticated invocations to [example-frontend] (y/N)?  y

    Deploying container to Cloud Run service [example-frontend] in project [example-frontend] region [us-west1]
    ✓ Deploying new service... Done.
    ✓ Creating Revision... Revision deployment finished. Waiting for health check to begin.
    ✓ Routing traffic...
    ✓ Setting IAM Policy...
    Done.
    Service [example-frontend] revision [example-frontend-00001-xoc] has been deployed and is serving 100 percent of traffic at https://example-frontend-s3sffdgxoq-uw.a.run.app
```

Finalmente, abre la URL de servicio en un navegador web para visitar el contenedor implementado.

```bash
https://example-frontend-s3sffdgxoq-uw.a.run.app
```

## Referencias

https://cloud.google.com/community/tutorials/deploy-react-nginx-cloud-run
