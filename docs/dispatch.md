# Dispatch: Join the DAMF with ease

`Dispatch` is an Intermediary tool for publishing, retrieval, and trust analysis in the Distributed Assertion Management Framework (DAMF).

## Obtaining and Building

### Source

- [Zip][dispatch-zip]
- [Github][dispatch-repo]

[dispatch-zip]: https://github.com/distributed-assertions/dispatch/archive/refs/heads/main.zip
[dispatch-repo]: https://github.com/distributed-assertions/dispatch

### Requirements

- [`Kubo`][kubo] - previously known as `ipfs-go`
- [`nodejs`][nodejs]

[kubo]: https://github.com/ipfs/kubo
[nodejs]: https://nodejs.org/en/

### Building

In the root directory of `Dispatch`, run:

>     npm install

Then run:

>     npm run build

Generated executables for `linux`, `macOS`, and `Microsoft Windows` shall be found at the `executables/` directory.

----------------

## Using

Once you have a built `Dispatch` executables, it is recommended to test the tool starting from the root directory. The executables are found in `/executables` directory, so by calling `./executables/dispatch-linux` (or use the Windows or the macOS executable as needed), you should get the list of available commands. You can use `-h` option to get more information about each command and its arguments.

In the rest of this document, we will use `dispatch` to stand for whichever executable you chose earlier.

### `publish` command

**set-up**

Before testing the `publish` command, you should:

- Create a PPK profile by: `dispatch keygen <profile-name>`. This profile name will be used to publish to IPFS (see below).
- For publishing to the cloud, create a free account on `web3.storage` and then create an _API token_ in order to use `dispatch` to access it. The `web3.storage` service is one way to make your data more quickly discoverable in the IPFS network. You must then set this API token using the command `dispatch set-web3token <token>`.

**publish**

There are some examples of the `.json` standard format input to `dispatch` in the `examples/input` directory. You can test publishing with these. For example:

>   dispatch publish fileA <your-profile-name> examples/input local

the above command should publish `fileA.json` with the sequents signed with your created profile, and these files are only stored on your local machine as you used the argument `local` for `<target>` parameter. If you want to publish through `web3.storage` service with your API token, use `cloud` instead of `local`.

(In the examples, you should publish `fileA` before `fileB` since `fileB` imports the sequence CID for `fileA`.)

### `get` command

**set-up**

For getting a CID through IPFS, there are usually 2 main ways: either through your own IPFS node connecting through the IPFS network or through a gateway which itself connects through the IPFS network. With our implementation, you have both options. The first option is to activate the IPFS daemon using the command `ipfs daemon` (in your terminal) and let your node try to discover your data through its IPFS network peers.

The other option is using a gateway. If you keep the daemon deactivated or if it were not able to retrieve the data for some reason, `dispatch` will try to retrieve the data through a `gateway`. The default gateway is set to "https://dweb.link". You can change it using the `dispatch set-gateway <gateway>` command (but it has to support the _IPFS DAG API_). Note that we haven't dealt directly with [verification of data when getting it from a gateway](https://docs.ipfs.tech/reference/http/gateway/#specifications) but it is something to do in the future.

**get**

After doing the publishing step, you should get a CID. You can use this CID for testing the `get` command **OR** you can use a CID from the list of already published CIDs in the `examples/cidList.md` file corresponding to the files of `examples/fib`. For example:

>    dispatch get bafyreicanspggit6yl57rmkouw4atbw4oa5mec573ikkwy3ga2zhxbbwr4 examples/output
