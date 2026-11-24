# disorderly mint

A Windows NFT mint bot for Afterimage holders. It watches a drop, prepares your transaction while it waits, and sends it the moment the sale opens.

This repository only holds the installers and the update feed. The source code is private.

**Download:** [disorderly.ai/bot](https://disorderly.ai/bot), or the newest file under [Releases](https://github.com/FITCH-B/disorderly-bot-releases/releases/latest).

## What you need

- Windows 10 or 11, 64-bit
- 5 Afterimages in any wallet you own. A cold wallet or a Ledger is fine. Nothing gets moved.
- One or more minting wallets, funded with ETH on the network you are minting on. These do not need to hold any Afterimages.

## Install

1. Download `disorderly-mint-Setup-<version>.exe` from the latest release.
2. Run it and pick where to install.
3. Open **disorderly mint** from the Start menu.

Only install files from this repository or from disorderly.ai/bot. disorderly.ai/bot shows the newest installer's SHA-256 fingerprint, and recent release notes list it too. To check a download, open PowerShell in your Downloads folder and run `Get-FileHash .\disorderly-mint-Setup-<version>.exe`. Any other value: delete the file.

**The installer is not code signed yet.** Our certificate is still being issued, so Windows SmartScreen says it protected your PC and the publisher shows as Unknown publisher. Click **More info**, check the file name, then **Run anyway**. Once signed releases start, installers will show **DISORDERLY LLC** as the publisher.

New to all this? The [beginner setup guide](https://disorderly.ai/bot/guide.html) walks through every click.

## Sign in

The app stays locked until you sign in with the wallet that holds your Afterimages.

1. The app opens behind a sign-in box. Click **Sign in with holder wallet**.
2. Your browser opens a sign-in page on disorderly.ai showing a code. Check it matches the code in the app.
3. Connect the wallet that holds your Afterimages (MetaMask, Rabby, Coinbase Wallet, or a Ledger through one of them) and sign the message. It costs no gas and gives no access to your funds.
4. Go back to the app. It unlocks within a few seconds.

Rather not connect your wallet? On the sign-in page, choose **Verify with a transaction instead**: you send 0 ETH from the holder wallet to itself with the code attached, on Ethereum, Base or Robinhood Chain. It costs a small network fee. A Safe works on Ethereum.

Good to know:

- Access is checked about every hour. If the holder wallet drops below 5 Afterimages, arming locks. Mints already armed still finish.
- One holder wallet can be signed in on two computers. Signing in on a third signs out the one used least recently.
- If disorderly.ai cannot be reached, the app stays unlocked for 24 hours after its last good check.
- Your settings and watchlist follow you to your other computer. Keys and RPC URLs never leave the computer they are on.

## Set up

### Minting wallets

Open the **Wallet** tab. Either:

- **Create new wallets:** pick how many (1 to 10) and a name, then click **Create**. They are generated on your computer. Click **Back up** on each one before you fund it: it copies the key to your clipboard for 30 seconds, then wipes it. A wallet that is never backed up is lost if the computer is.
- **Add one you have:** give it a label (for example `main` or `AL-1`), paste its private key, and click **Add**.

Keys are encrypted by Windows on your computer. The app can use a key to sign but never shows it in its window. Use dedicated hot wallets for minting, never the wallet with your Afterimages or savings.

**Fund wallets** sends the same amount of ETH from one app wallet to several others on a network, with the total and fees shown before anything is sent. Only wallets in the app can receive it.

**Sweep to home** sends the NFTs the app minted, then leftover ETH, to your home address (your holder or cold wallet). The home address stays on your computer and changing it asks first.

Some projects ban minting from several wallets. Check each drop's rules.

### Settings (optional, but faster)

Open the **Settings** tab.

- **RPC per network:** paste your own RPC URL (Alchemy, QuickNode, your own node) for the networks you mint on. Click **Test** to check it, and **Compare** to race it against the public RPC: response time, and which one sees new blocks first. Without one, the app uses a public RPC.
- **Etherscan API key:** needed to load contracts on Ethereum, Base, Arbitrum, Optimism and Polygon. Robinhood Chain needs no key.
- **Anthropic API key:** optional. Lets Claude read a contract's source and flag anything risky before you arm.
- **Limits:** daily spend cap, max transactions per day, how many reverts in a row before it stops, and default max fees. Every limit is checked right before signing.
- **Flashbots Protect:** on by default for Ethereum. A mint that would fail is dropped instead of costing gas.
- **Mint alerts:** desktop notifications, and a post in your own Discord channel when a mint lands. Paste a Discord webhook URL (channel settings, Integrations, Webhooks), click **Test**, then **Save**.
- **Phone companion:** turn it on and scan the QR code with your phone to see armed mints, get alerts and Stop all from anywhere. The phone can never arm, see a key or change settings. End-to-end encrypted.

## Mint

### 1. Watch the contract

On the **Watch** tab, paste the contract address, an explorer link, or an OpenSea item link, pick the network (or leave it on auto), and click **Watch**.

The app finds the mint function, the price, supply and per-wallet limit, and the sale status (live, countdown, closed, paused or sold out). OpenSea drops are minted through SeaDrop automatically.

If the contract is not verified on the explorer, click **paste ABI instead** and paste the contract's ABI.

### 2. Check it

- **Simulate** runs the mint against the chain without sending anything and shows whether it would succeed.
- **Read contract** asks Claude to review the source, if you added an Anthropic key.

### 3. Arm it

In the arm panel on the right, choose:

- **Wallet:** which minting wallet sends it
- **Phase:** public, or allowlist with the Merkle proof the project gave that wallet
- **Quantity**
- **Max total (ETH):** the most this mint may spend, not counting gas
- **Max fee (gwei):** the highest gas price you accept

Click **Arm**. From then on the app:

- keeps the transaction prepared while it waits, so the send is a single step
- on Ethereum, also sends it straight to the block builders, with a tip set above recent blocks and rival mints, and resends it with a higher tip if it is not picked up
- if the sale has a published start time, sends it on a timer aimed at the first block of the sale, and checks ahead of time that the mint will succeed then
- otherwise sends it the first block the sale is open
- disarms after it lands, so it never mints twice. A timed send that lands a moment before the sale opens gets one retry.

Keep the app open and the computer awake until the sale. Restarting the app disarms everything.

To mint more from several wallets, arm each wallet separately. **Mint now** sends straight away if the sale is already open. **Disarm** cancels an armed mint.

### 4. Follow it

The event stream at the bottom shows every step: arming, sending, the receipt, and how many tokens actually reached your wallet. The **Activity** tab keeps the full history.

## Stop everything

Click **Stop all** at the top right, or press **F12**. Every armed mint is disarmed and nothing can be sent until you click **Resume**.

## Updates

The app only installs updates signed by DISORDERLY LLC. Until signed releases start, it cannot update itself: download the newest installer from disorderly.ai/bot and run it over your current version. Your settings, wallets and watchlist stay. Once releases are signed, updates download in the background and install when you restart.

## Supported networks

Ethereum, Robinhood Chain, Base, Arbitrum, Optimism, Polygon, plus Sepolia and Robinhood testnet for practice.

## Troubleshooting

- **The app says it is locked:** click **Sign in with holder wallet** in the sign-in box. If it says the wallet holds fewer than 5 Afterimages, switch to the right wallet in your browser and sign in again.
- **"No wallet found" on the sign-in page:** open the page in a browser with a wallet extension, or in your wallet app's built-in browser.
- **A contract will not load:** add an Etherscan key in Settings, or paste the ABI.
- **"Simulation reverted":** the mint would fail right now. The reason is shown next to it. Common ones are a sale that is not open yet, the wrong price, or a per-wallet limit already reached.
- **A wallet is marked as a smart account (EIP-7702):** it can only have one pending transaction at a time. Use a plain wallet for each mint.

## Safety

Minting is risky. Contracts can be malicious, and fees are spent whether or not you get a token. Only arm mints you have checked, and fund each minting wallet only with what that mint needs. Nothing here is financial advice.

## Contact

Updates and support: [@disorderly_AI on X](https://x.com/disorderly_AI).

(c) 2026 DISORDERLY LLC
