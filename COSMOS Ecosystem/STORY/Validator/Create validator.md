

# Create validator

## Export private key
By default, when you run 
`story init` a `validator key` is created for you. To view `your validator key`, run the following command:

```
story validator export
```

In addition, if you want to export the derived `EVM private key` of your validator into the default data config directory, please run the following:

```
story validator export --export-evm-key
```
![image](https://github.com/user-attachments/assets/192dcca2-c7f8-4b60-a9c2-e6c2e53d0f60)


***Important: Please keep your private key in a safe place**

Your EVM Private Key saved to: `/root/.story/story/config/private_key.txt`

Note that to participate in consensus, at least `1 IP` must be staked (equivalent to `1000000000000000000 wei`
)!

Faucet link: 
```
https://faucet.story.foundation/
```

```
story validator create --stake 1000000000000000000 --private-key "your_private_key"
```
![image](https://github.com/user-attachments/assets/6481f649-f7e0-4520-8aec-42daf9078239)


### Backup the validator key:

File location: `/root/.story/story/config/priv_validator_key.json`

**Copy this file to your local machine.**

Store it carefully; this is the most crucial key for your validator.

# END
