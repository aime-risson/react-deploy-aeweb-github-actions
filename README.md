# Deploy to AEweb GitHub action

This is a very simple GitHub action that allows you to deploy to Archethic decentralised webhosting.
The action works by running part of the [aeweb-cli](https://github.com/archethic-foundation/aeweb-cli) code.

## Usage

To get started using the action, create a folder called .github and inside it, create another folder called workflows.
Finally inside the workflows folder, create a file called main.yml with the following contents:

```yaml
on: [push]

jobs:
  deploy_to_aeweb:
    runs-on: ubuntu-latest
    name: A job to deploy file to AEweb
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Deploy to AEweb
        id: deploy
        uses: archethic-foundation/aeweb-github-action@v1.7.1
        with:
          seed: ${{ secrets.ARCH_BASE_SEED }}
          endpoint: "https://testnet.archethic.net" #Endpoint you want to deploy to
          path: "./web_site_test" #Path to the folder you want to deploy
```

In your Repo, go to Settings -> Secrets and click on "New Secret". Then enter *ARCH_BASE_SEED* as the seed to your transaction chain.

**The seed should never be hardcoded in your code.**

Please make sure you have sufficient funds in your chain to deploy your file/folder.

## Keychain's integration

if you want to leverage your decentralized keychain you can use one your service to deploy website.

```yaml
on: [push]

jobs:
  deploy_to_aeweb:
    runs-on: ubuntu-latest
    name: A job to deploy file to AEweb
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Deploy to AEweb
        id: deploy
        uses: archethic-foundation/aeweb-github-action@v1.7.1
        with:
          seed: ${{ secrets.ARCH_BASE_SEED }}
          endpoint: "https://testnet.archethic.net" #Endpoint you want to deploy to
          path: "./web_site_test" #Path to the folder you want to deploy
          keychainFundingService: "archethic-wallet-TEST_AEWEB" # Service to fund the website's chain
          keychainWebsiteService: "aeweb-website1" # Service to identify the website's chain
```

If you want to use the wallet, you have to provide your passphrase composed of 24 words i the Github's secret: *ARCH_BASE_SEED*

## SSL integration

if you wish to deploy your website to HTTPS, we have to include a SSL certificate and SSL private key in the Github Actions secrets and 
AEWeb will deploy the website securely on the Archethic Blockchain.

```yaml
on: [push]

jobs:
  deploy_to_aeweb:
    runs-on: ubuntu-latest
    name: A job to deploy file to AEweb
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Deploy to AEweb
        id: deploy
        uses: archethic-foundation/aeweb-github-action@v1.7.1
        with:
          seed: ${{ secrets.ARCH_BASE_SEED }}
          endpoint: "https://testnet.archethic.net" #Endpoint you want to deploy to
          path: "./web_site_test" #Path to the folder you want to deploy
          sslCertificateFile: "cert.pem" # Filepath for the certificate file
          sslKey: ${{ secrets.SSL_KEY }}


```

In your Repo, go to Settings -> Secrets and click on "New Secret". Then enter *SSL_KEY* as credentials for your HTTPS website.

**The SSL key should never be hardcoded in your code.**


## Notes

Be careful, the action does not yet support max fees limitation. 
More your website will be big and how many updates you will do, more you will need to fund the website's chain. 

## Development

To test the development of the Github actions, you can use docker container.

```sh
docker build -t aeweb_actions .
docker run -e INPUT_SEED=XXX -e INPUT_ENDPOINT=https://testnet.archethic.net -e INPUT_PATH=XXX aeweb_actions
```

