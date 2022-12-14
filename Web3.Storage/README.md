# Web3.Storage - Node.js

Examples for using the web3.storage client from Node.js.

- `put-files.js` - Adds web files to web3.storage
- `put-files-from-fs.js` - Adds files from your file system to web3.storage
- `put-car-from-fs.js` - Adds a Content Archive (CAR) file from your file system to web3.storage
- `put-car-dag-cbor.js` - Creates a Content Archive in memory from IPLD data and adds to web3.storage

<h3>Stepts to upload files to IPFS</h3>

1. Install: `npm i web3.storage`

2. Sign in to https://web3.storage, create an API token, and use it in place of API_TOKEN when creating your instance of the client.
3. Create a file called:

**put-files.js**

```
import process from 'process'
import minimist from 'minimist'
import { Web3Storage, getFilesFromPath } from 'web3.storage'

async function main () {
const args = minimist(process.argv.slice(2))
const token = args.token

if (!token) {
return console.error('A token is needed. You can create one on https://web3.storage')
}

if (args.\_.length < 1) {
return console.error('Please supply the path to a file or directory')
}

const storage = new Web3Storage({ token })
const files = []

for (const path of args.\_) {
const pathFiles = await getFilesFromPath(path)
files.push(...pathFiles)
}

console.log(`Uploading ${files.length} files`)
const cid = await storage.put(files)
console.log('Content added with CID:', cid)
}

main()
```

4. Create another file called:

**package.json**

```
{
    "name": "web3-storage-quickstart",
    "version": "0.0.0",
    "private": true,
    "description": "Get started using web3.storage in Node.js",
    "type": "module",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "dependencies": {
        "minimist": "^1.2.5",
        "web3.storage": "^3.1.0"
    },
    "author": "YOUR NAME",
    "license": "(Apache-2.0 AND MIT)"
}
```

5. Once we have added the JSON, we need to install all the dependencies, we just type on the terminal: `npm install `
6. Run the script by calling `node put-files.js`, using `--token`  to supply your API token and specifying the path and name of the file you want to upload. If you'd like to upload more than one file at a time, simply specify their paths/names one after the other in a single command. Here's how that looks in template form:

- `node put-files.js --token=<YOUR_TOKEN> <filename1> ~/filename2 ~/filenameN`

You also can run...

- `node put-files-from-fs.js --token=<YOUR_TOKEN>`
- `node put-car-from-fs.js --token=<YOUR_TOKEN>`
- `node put-car-dag-cbor.js --token=<YOUR_TOKEN>`

7. This will return us a CID.

- Example: `Content added with CID: bafybeidscipod5zke2jry6fbbsopbo5alihrqe56cuzhswxbrkljocetlm`

8. Go to `https://dweb.link/ipfs/YOUR_CID`, replacing YOUR_CID with the CID you noted in the last step.

- Example:
  `https://dweb.link/ipfs/bafybeidscipod5zke2jry6fbbsopbo5alihrqe56cuzhswxbrkljocetlm`
