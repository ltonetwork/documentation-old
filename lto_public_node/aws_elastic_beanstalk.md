Take to following steps to install the node on EB:

1. Zip the Dockerrun.aws.json file
2. Create an application
3. Inside the created application, create an environment: `webserver environment`
4. Select following settings:
  - Platform: Docker
  - Upload the zipped file
5. Configure more options
6. Instances -> Instance type: Choose an instance with atleast 2 gb of memory (E.g. t2.small)
7. Software -> Environment properties:
    - Name: `LTO_WALLET_PASSWORD`, Value: `Your wallet password`
    - Name: `LTO_WALLET_SEED` or `LTO_WALLET_SEED_BASE58`, Value: `Wallet Seed`

Now your node is should good to go!
