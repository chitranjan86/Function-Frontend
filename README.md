# Function-Frontend
 a simple contract with 2-3 functions.
# Simple Storage Smart Contract with Frontend

This project demonstrates a simple Ethereum smart contract for storing and retrieving data, along with a frontend application to interact with the contract.

## Project Structure

- `SimpleStorage.sol`: The Solidity smart contract.
- `index.html`: The frontend application to interact with the smart contract.
- `web3.min.js`: Web3.js library to connect to the Ethereum blockchain.

## Prerequisites

- [Node.js](https://nodejs.org/)
- [MetaMask](https://metamask.io/) browser extension

## Steps to Run the Project

### Step 1: Smart Contract

1. Open [Remix IDE](https://remix.ethereum.org/).
2. Create a new file named `SimpleStorage.sol`.
3. Copy and paste the following Solidity code into the file:

    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    contract SimpleStorage {
        uint256 private storedData;
        string private storedString;

        // Function to set an integer
        function set(uint256 x) public {
            storedData = x;
        }

        // Function to get the stored integer
        function get() public view returns (uint256) {
            return storedData;
        }

        // Function to set a string
        function setString(string memory str) public {
            storedString = str;
        }

        // Function to get the stored string
        function getString() public view returns (string memory) {
            return storedString;
        }
    }
    ```

4. Compile the contract using the Solidity compiler.
5. Deploy the contract using the "Deploy & Run Transactions" tab.
6. Copy the deployed contract address for use in the frontend.

### Step 2: Frontend Application

1. Create an `index.html` file and copy the following HTML and JavaScript code into it:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Simple Storage</title>
        <script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
    </head>
    <body>
        <h1>Simple Storage Contract</h1>
        <div>
            <label for="value">Set Value:</label>
            <input type="number" id="value" />
            <button onclick="setValue()">Set</button>
        </div>
        <div>
            <label for="str">Set String:</label>
            <input type="text" id="str" />
            <button onclick="setString()">Set</button>
        </div>
        <div>
            <button onclick="getValue()">Get Value</button>
            <p>Stored Value: <span id="storedValue"></span></p>
        </div>
        <div>
            <button onclick="getString()">Get String</button>
            <p>Stored String: <span id="storedString"></span></p>
        </div>

        <script>
            // Ensure you have MetaMask installed and are connected to a network
            const web3 = new Web3(Web3.givenProvider || "http://localhost:8545");

            // Replace this with your deployed contract address
            const contractAddress = "YOUR_CONTRACT_ADDRESS";
            const contractABI = [
                {
                    "inputs": [
                        {
                            "internalType": "uint256",
                            "name": "x",
                            "type": "uint256"
                        }
                    ],
                    "name": "set",
                    "outputs": [],
                    "stateMutability": "nonpayable",
                    "type": "function"
                },
                {
                    "inputs": [],
                    "name": "get",
                    "outputs": [
                        {
                            "internalType": "uint256",
                            "name": "",
                            "type": "uint256"
                        }
                    ],
                    "stateMutability": "view",
                    "type": "function"
                },
                {
                    "inputs": [
                        {
                            "internalType": "string",
                            "name": "str",
                            "type": "string"
                        }
                    ],
                    "name": "setString",
                    "outputs": [],
                    "stateMutability": "nonpayable",
                    "type": "function"
                },
                {
                    "inputs": [],
                    "name": "getString",
                    "outputs": [
                        {
                            "internalType": "string",
                            "name": "",
                            "type": "string"
                        }
                    ],
                    "stateMutability": "view",
                    "type": "function"
                }
            ];

            const simpleStorage = new web3.eth.Contract(contractABI, contractAddress);

            async function setValue() {
                const accounts = await web3.eth.requestAccounts();
                const value = document.getElementById("value").value;
                await simpleStorage.methods.set(value).send({ from: accounts[0] });
                alert("Value set successfully");
            }

            async function getValue() {
                const value = await simpleStorage.methods.get().call();
                document.getElementById("storedValue").innerText = value;
            }

            async function setString() {
                const accounts = await web3.eth.requestAccounts();
                const str = document.getElementById("str").value;
                await simpleStorage.methods.setString(str).send({ from: accounts[0] });
                alert("String set successfully");
            }

            async function getString() {
                const str = await simpleStorage.methods.getString().call();
                document.getElementById("storedString").innerText = str;
            }
        </script>
    </body>
    </html>
    ```

2. Replace `"YOUR_CONTRACT_ADDRESS"` with the actual address of your deployed contract.

### Step 3: Running the Application

1. Open the `index.html` file in your web browser.
2. Ensure you have MetaMask installed and connected to the appropriate network.
3. Use the web interface to interact with the smart contract:
   - Set and get integer values.
   - Set and get string values.

## Summary

This project demonstrates how to create a simple smart contract using Solidity and interact with it using a web frontend built with HTML, JavaScript, and Web3.js. By following the steps above, you can set and retrieve values from the Ethereum blockchain through a user-friendly interface.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
