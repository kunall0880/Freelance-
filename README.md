# **Delance: A decentralised freelancing platform**

We have tried to make a **blockchain-based decentralised web application (DApp)** that allows freelancers and recruiters (clients) to connect on projects with built-in escrow. The recruiter (client) can put their project ideas and set the guidelines for the same. The freelancer can browse through the available projects on the platform and apply for the ones they are interested in. The freelancer will have to upload their files as a proof of work for each milestone that is set by the client.

## Unique Selling Point: 
The existing platforms don't have an **automated rating system**. Our platform automates the rating system so as to **prevent ratings being abused by malicious actors**.
There also does not exist a comprehensive solution which automates the entire workflow and integrates it with **decentralized arbitration** as we have done. 
The files are also uploaded on **IPFS** making them available as an **immutable proof** for the arbitrators. We have also **automated the payment of funds** after acceptance of proof of work for each milestone via **escrow contracts**.

## Workflow:

- Our website begins with asking the user to login/sign up as a client or freelancer.
  
### Client Side:
- The client dashboard displays details about the client's **Etherium (MetaMask)** account. The client can either add or view projects. For adding a project, the client must fix the reward amount.
- Client can then split up the work of each project into different milestones. Each milestone specifies the percentage of reward to be transacted upon completion of that milestone by the freelancer.
- The client can also view all the pending requests that the freelancers might have sent and can either decline or accept. The request contains important details like the freelancers address and rating.
- If accepted, a new **escrow account** is created, and the total reward is transferred from the client's account to the escrow account. All the future milestone payments are done from this escrow account.

### Freelancer Side:
- The freelancer can view all the projects that are available. Each project contains the necessary details, along with the option of applying for the project and viewing the milestones. Once accepted, the freelancer can start uploading the necessary files as proof for each milestone. Each file is stored on **IPFS** through a **dedicated gateway** (QuickNode Service), and a **CID (Content Identifier)** is generated for the same.
  
- The client views the proof files submitted by the freelancer for a particular milestone. If the client accepts the submission, the rating of the freelancer will get incremented, and the reward allotted for this milestone will be credited to the freelancer's account. If the client rejects the submission, the freelancer can either accept the rejection and work on it again (rating of freelancer decreases) or call for an **arbitration service (through Kleros)**.

The web application works smoothly on localhost (utilizing Ganache Etherum accounts). The contracts have also been deployed on Sepolia public testnet, through an Etherum RPC Endpoint. 


## Features

1. **Client Project Posting**: Clients can create project requests with:
   - Project name and description
   - Payment amount
   - Milestones and payments for each milestone

2. **Freelancer File Submission**: Freelancers view available projects, select one to work on, and submit files at each milestone.

3. **Automated Payment and Rating System**: 
   - **Case 1**: If the client accepts the submitted files, payment is released automatically, and ratings are updated.
   - **Case 2**: If the client accepts with delayed milestones, the payment is reduced, and the freelancer's rating decreases slightly.
   - **Case 3**: If the client rejects the submission, the freelancer has the option to either revise the files or raise a dispute.
       - **Subcase 1**: Freelancer agrees to revise the work and resubmits.
       - **Subcase 2**: Freelancer raises a dispute; Kleros appoints an anonymous arbitrator to resolve the issue, determining the fund distribution.

4. **Dispute Resolution via Kleros**: When disputes arise, the Kleros arbitration system ensures fair decision-making.

5. **Rating System**: Ratings are calculated as a weighted average of all transaction outcomes, considering factors like transaction success or delay.

## Technology Stack

- **Ethereum (Sepolia test network)**: Smart contract deployment
- **Kleros**: Decentralized arbitration for disputes
- **IPFS**: Decentralized file storage
- **React**: Frontend for the user interface
- **Ethers.js**: Interact with the Ethereum network and contracts

## Prerequisites

To run this project, ensure you have the following installed:
- **Node.js** and **npm**: Download from [Node.js Official Website](https://nodejs.org/).
- **MetaMask**: Browser wallet for Ethereum interaction.
- **QuickNode Account**: Ethereum and IPFS provider, available at [QuickNode](https://www.quicknode.com/).
- **Ganache Account**: App for test accounts and network.

## Installation and Setup

1. **Clone the Repository**:
   ```bash
   https://github.com/sassy2711/Delance.git
   cd Delance

2. **Install MetaMask**:
- Install MetaMask from the [MetaMask website](https://metamask.io/).

3. **Connecting MetaMask with ganache**:
- Start ganache network.
- Choose new workspace, give whatever name.
- Click on add project, go to Delance/truffle_project, select the truffle-config.js file and then save the project(in ganache).
- Copy the private key of an account.
- Go to your metamask wallet.
- Click on add account.
- Click on import wallet.
- Paste the private key.
- The account is made.
- Again go on the metamask wallet, connect the account.
- Refresh at each step just in case.

4. **Compile and Deploy Smart Contracts**

   Ganache must be running before you deploy. You can use the GUI or CLI.
   If you use the CLI install the package and start it with:

   ```bash
   cd Delance
   npm run ganache           # starts Ganache on 127.0.0.1:7545
   # or run `npx ganache -p 7545` directly
   ```

   In a second terminal launch the migration process:

   ```bash
   cd Delance/truffle_project
   truffle migrate --network development --reset
   ```

   The `--network development` flag points to the Ganache configuration in
   `truffle-config.js` (chain ID 1337). The `--reset` ensures contracts are
   redeployed each time.

5. **Copy the contract artifacts**

   After a successful migration, Truffle generates JSON artifacts in
   `Delance/truffle_project/build/contracts/`. Copy them into the React source
   directory so the frontend can access ABIs and addresses:

   ```bash
   cp truffle_project/build/contracts/*.json src/contracts/
   ```

   (Windows command equivalent shown earlier.)

6. **Configure MetaMask**

   - Open MetaMask and click the network selector at the top.
   - Choose **Add network** → **Add a custom network**.
   - Fill in the values:
     - Network name: `Ganache`
     - RPC URL: `http://127.0.0.1:7545`
     - Chain ID: `1337` (check your Ganache output if different)
     - Currency symbol: `ETH`
   - Save and **select Ganache** as the active network.
   - Import one of the accounts listed in Ganache using its private key.

   **Important:** if MetaMask is pointed at any other network (e.g. Sepolia or
   Base), the app will prompt for gas and fail to locate the contracts. Always
   switch to the Ganache network while developing locally.

7. **Run the Frontend**

   Install dependencies and start React:

   ```bash
   cd Delance
   npm install
   npm start
   ```

   The app will open at `http://localhost:3000`. Connect MetaMask when
   prompted; the network ID and contract addresses are logged to the browser
   console for debugging.

8. **Adding/New Projects**

   Use the client UI to add projects. Transactions will be mined instantly by
   Ganache with no real Ether cost. Refresh the page or reopen the app if the
   contract address or network changes. The `web3.js` module automatically
   resolves the correct contract address based on the active network ID.

---

## Additional Scripts

The `package.json` in the root of `Delance` includes a few helpful commands:

```json
{
  "scripts": {
    "ganache": "ganache -p 7545",
    "migrate:dev": "truffle migrate --network development --reset",
    "test:dev": "truffle test --network development"
  }
}
```

Use `npm run ganache` to launch the local chain, `npm run migrate:dev` to
redeploy and `npm run test:dev` to execute the Solidity test suite locally.

## Environment Variables

The project uses a `.env` file (Gitignored) to store sensitive values. For
local development you only need an optional mnemonic. Two variables can
override contract addresses when connecting to other networks:

```env
REACT_APP_PROJECTS_CONTRACT_ADDRESS=0x...
REACT_APP_REQUEST_MANAGER_ADDRESS=0x...
```

These are read by `src/services/web3.js` and take precedence over the artifact
lookup.

## Troubleshooting

- **Contracts not found / "Unable to determine Projects contract address"**
  - Ensure MetaMask is on the Ganache network (chain ID 1337). Look at its
    network dropdown; if it says anything else, switch networks or re-add
    Ganache.
  - Confirm the JSON artifact you copied contains the `networks` block with
    the correct chain ID and address. Redeploy and recopy if necessary.
  - Check the browser console for the log messages added by `getDeployedAddress`.

- **Ganache CLI not recognized**
  - Use `npm install` inside `Delance` to add `ganache` binary. The script
    `npm run ganache` should then work.
  - You can also install `ganache` globally: `npm install -g ganache`.

- **Chain ID mismatch (5777 vs 1337)**
  - Update `truffle-config.js` to match the `chainId` reported by Ganache, or
    run Ganache with `--chainId 1337`.

- **MetaMask refuses transaction from localhost**
  - Make sure the request origin (`http://localhost:3000`) is allowed in
    MetaMask settings under "Connected sites" and you have unlocked the
    imported account.

## Deploying to a Testnet or Mainnet

When you're ready to move off Ganache, configure a network entry in
`truffle-config.js` with an HDWallet provider and appropriate RPC URL. Set the
environment variables `MNEMONIC` and, for example, `SEPOLIA_RPC_URL` or
`INFURA_API_KEY`. The frontend will then pick up addresses via the artifacts
(or env overrides if provided).

---

## Gitignore

The project already includes a `.gitignore` file to keep sensitive or
machine-specific files out of version control. Here's a sample you can
reference or extend:

```
# React build output
/node_modules/
/build/
/dist/

# local env files
.env

# Truffle build artifacts (optional if you copy to src/contracts)
/truffle_project/build/contracts/

# IDE/editor directories
.vscode/
.idea/

# log files
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# OS files
.DS_Store
Thumbs.db
```

Make sure your own `.env` never gets committed; it contains your mnemonic
and RPC URLs. You can add additional entries such as `*.secret` or other
private data.

## Links

- [Truffle documentation](https://www.truffleframework.com/docs/)
- [Ganache CLI](https://www.trufflesuite.com/ganache)
- [MetaMask custom RPC guide](https://metamask.io/faqs/#c6)

Happy hacking! Feel free to file issues or pull requests in this repo if you
want to refine the setup or add automation.  

## Running the Application

### Start the Development Server

    npm start

### Open the DApp

- Open your browser and navigate to `http://localhost:3000`.

### Dispute Resolution

1. If a freelancer raises a dispute, an anonymous arbitrator from Kleros is appointed to review the case.
2. Based on the arbitrator's decision:
   - If the client’s rejection is validated, funds remain in the contract.
   - If the freelancer's submission is upheld, funds are released to the freelancer.
3. **Automatic Rating Adjustment**:
   - Ratings for both the client and freelancer are updated based on the arbitration outcome, influencing each party’s reputation on the platform.





