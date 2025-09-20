Overview
This project demonstrates a msg.value reuse vulnerability in a Multicall implementation. By batching two deposit() calls in one multicall transaction with only 1 ETH, an attacker’s internal balance is credited with 2 ETH. The PoC includes both a unit test and a script to reproduce the exploit on a local fork.

Prerequisites:
Foundry toolchain installed (forge, anvil)

Git installed


Project Structure

forge-poc-templates/
├── foundry.toml
├── lib/
│   └── forge-std/           # dependencies installed here
├── src/
│   └── base/
│       └── Multicall.sol    # your vulnerable contract
├── script/
│   └── RunExploit.s.sol     # your exploit script
├── test/
│   └── MulticallExploit1.t.sol  # your test PoC
└── README.md



Setup:

1.Clone this repo
  git clone <your-repo-url>
cd <repo-directory>


2.Install dependencies
 forge install forge-std


Running the PoC

1. Unit Test PoC
   Runs a Foundry unit test demonstrating the exploit assertions.

forge test --match-path test/MulticallExploit1.t.sol -vv

Expected output:

[⠆] Compiling...
No files changed, compilation skipped

Ran 1 test for test/MulticallExploit1.t.sol:MulticallExploitTest
[PASS] testMsgValueReuseAttack() (gas: 66731)
Suite result: ok. 1 passed; 0 failed; 0 skipped; finished in 21.94ms (4.75ms CPU time)

Ran 1 test suite in 195.86ms (21.94ms CPU time): 1 tests passed, 0 failed, 0 skipped (1 total tests)

2. Script PoC
 Runs a Forge script on a local Anvil fork, printing console logs of each step.

  1.Start a local fork with Anvil:
    anvil
  2.In a new terminal, run the exploit script:
   forge script script/RunExploit.s.sol --tc RunExploit --fork-url http://127.0.0.1:8545
# the anvil what we started is Listening on 127.0.0.1:8545
# so in forge test for script/RunExploit.s.sol we ised fork url as 127.0.0.1:8545

Expected output:

[⠊] Compiling...
No files changed, compilation skipped
Warning: Detected artifacts built from source files that no longer exist. Run `forge clean` to make sure builds are in sync with project files.
 - /home/ubuntu/forge-poc-templates/test/MulticallExploit.t.sol
Script ran successfully.

== Logs ==
  === Exploit PoC Start ===
  Attacker address: 0x7E5F4552091A69125d5DfCb7b8C2659029395Bdf
  Attacker initial ETH balance: 1000000000000000000
  Vault deployed at: 0x5aAdFB43eF8dAF45DD80F4676345b7676f1D70e3
  Vault recorded attacker balance: 2000000000000000000
  Attacker final ETH balance: 2000000000000000000
  === Exploit PoC End ===


Notes:
No --broadcast flag is used; all executions run in simulation mode on a local fork.

Both the unit test and script PoC demonstrate the same vulnerability.

Ensure solc_version in foundry.toml is set to 0.8.13 to match the contract code.
