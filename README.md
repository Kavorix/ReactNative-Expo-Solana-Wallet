# Solana Wallet Cross-Platform App with Expo, Web3 & React Native

## Screens and Features

### Welcome

This screen only shows a button to start!

![Welcome](https://github.com/Kavorix/ReactNative-Expo-Solana-Wallet/blob/main/screenshots/welcome.png)




### Create your passcode

This screen let you create a passcode that for now is only requested when you want to see your recovery phrase.

Later, it can be used to encrypt the seed, before doing a transfer or even to access to the full app.


![Passcode](https://github.com/Kavorix/ReactNative-Expo-Solana-Wallet/blob/main/screenshots/passcode.png)


### Dashboard

This screen shows the account balance and soon it will show the Activity of the account.

Also, it is where I placed the Navigation using a floating action button


![Dashboard](https://github.com/Kavorix/ReactNative-Expo-Solana-Wallet/blob/main/screenshots/dashboard.png)


Get account balance with web3:

~~~javascript
const getBalance = async (publicKey) => {
  const connection = createConnection();
  const _publicKey = publicKeyFromString(publicKey);

  const lamports = await connection.getBalance(_publicKey).catch((err) => {
    console.error(`Error: ${err}`);
  });

  const sol = lamports / LAMPORTS_PER_SOL;
  return sol;
};
~~~

### Receive

This screen shows the address and a qr to make easier receive tokens.


![Receive](https://github.com/Kavorix/ReactNative-Expo-Solana-Wallet/blob/main/screenshots/receive.png)


### Send

This screen allows you to send tokens to other accounts typing the address or scanning a qr code.

Also, this screen shows the current price of SOL, SOL available in the account and convert the introduced amount to USD.

Validations are pending!

![Send](https://github.com/Kavorix/ReactNative-Expo-Solana-Wallet/blob/main/screenshots/send.png)

Create transaction with web3:

~~~javascript
const transaction = async (from, to, amount) => {
  const account = accountFromSeed(from.seed);

  console.log("Executing transaction...");
  console.log(amount);

  const transaction = new solanaWeb3.Transaction().add(
    solanaWeb3.SystemProgram.transfer({
      fromPubkey: publicKeyFromString(from.account),
      toPubkey: publicKeyFromString(to),
      lamports: amount * LAMPORTS_PER_SOL,
    })
  );

  // Sign transaction, broadcast, and confirm
  const connection = createConnection();
  const signature = await solanaWeb3.sendAndConfirmTransaction(
    connection,
    transaction,
    [account]
  );
  console.log("SIGNATURE", signature);
};

~~~

Get Solana price using [Coingecko](https://www.coingecko.com) API:

~~~javascript
const getSolanaPrice = async () => {
  const response = await fetch(
    `https://api.coingecko.com/api/v3/simple/price?ids=solana&vs_currencies=usd`,
    {
      method: "GET",
    }
  );

  const data = await response.json();
  return data.solana.usd;
};

~~~

### Settings

This screen shows two options:

![Settings](https://github.com/Kavorix/ReactNative-Expo-Solana-Wallet/blob/main/screenshots/setting.png)

#### Backup

To access to recovery phrase. Before ask for passcode.

#### Request Airdrop

This app is configured to connect to dev network so the tokens showed are not real.

Good thing is that every time you press here you get 1 SOL in your account that can be used to test the app, make transfers...

Request an Airdrop in dev mode with web3:

~~~javascript
const requestAirDrop = async (publicKeyString: string) => {
  const connection = createConnection();

  const airdropSignature = await connection.requestAirdrop(
    publicKeyFromString(publicKeyString),
    LAMPORTS_PER_SOL
  );

  const signature = await connection.confirmTransaction(airdropSignature);
  return signature;
};
~~~

## What I used to build this Solana Wallet

### Expo
Expo is an open-source platform for making universal native apps for Android, iOS, and the web with JavaScript and React.
 - [Expo](https://expo.io/)


### Solana/web3.js
This is the Solana Javascript API built on the Solana JSON RPC API.
 - [Solana/web3.js](https://solana-labs.github.io/solana-web3.js/)

### Easy Peasy
Vegetarian friendly state for React.
 - [Easy Peasy](https://easy-peasy.vercel.app/)

### React Native Paper
Paper is a collection of customizable and production-ready components for React Native, following Google’s Material Design guidelines.
 - [React Native Paper](https://callstack.github.io/react-native-paper/)

### React Navigation
Routing and navigation for Expo and React Native apps.
 - [React Navigation](https://reactnavigation.org/)


## Run it:

~~~bash
$ git clone https://github.com/Kavorix/ReactNative-Expo-Solana-Wallet
$ cd ReactNative-Expo-Solana-Wallet
$ yarn install
$ expo web or expo start
~~~
