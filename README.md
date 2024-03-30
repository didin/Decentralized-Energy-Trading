# Decentralized-Energy-Trading
Create a WEB3-based platform for peer-to-peer energy trading, allowing households with solar panels to sell excess energy to their neighbors.
# Simplified Demo for Decentralized Energy Trading Platform

class DecentralizedEnergyMarket:
    def __init__(self):
        # Simulates a blockchain ledger of transactions
        self.transactions = []
        # Simulates a registry of users and their balances (in a real scenario, balances could be energy credits)
        self.user_balances = {}
        # Listings of energy for sale: {seller_id: [(amount, price per unit), ...]}
        self.energy_listings = {}

    def register_user(self, user_id):
        if user_id not in self.user_balances:
            self.user_balances[user_id] = 100  # Assign an initial balance, for demo purposes
            print(f"User {user_id} registered with an initial balance of 100.")
        else:
            print("User already registered.")

    def list_energy(self, seller_id, amount, price):
        if seller_id not in self.energy_listings:
            self.energy_listings[seller_id] = []
        self.energy_listings[seller_id].append((amount, price))
        print(f"User {seller_id} listed {amount} units of energy for {price} per unit.")

    def buy_energy(self, buyer_id, seller_id, amount):
        listings = self.energy_listings.get(seller_id, [])
        for i, (available_amount, price) in enumerate(listings):
            if available_amount >= amount:
                total_cost = amount * price
                if self.user_balances[buyer_id] >= total_cost:
                    self.user_balances[buyer_id] -= total_cost
                    self.user_balances[seller_id] += total_cost
                    listings[i] = (available_amount - amount, price)
                    self.transactions.append((buyer_id, seller_id, amount, total_cost))
                    print(f"Transaction successful: {buyer_id} bought {amount} units of energy from {seller_id} for {total_cost}.")
                    return
                else:
                    print("Insufficient balance.")
                    return
        print("Requested amount not available.")

# Example usage
market = DecentralizedEnergyMarket()
market.register_user("Alice")
market.register_user("Bob")
market.list_energy("Alice", 50, 2)  # Alice lists 50 units of energy for 2 each
market.buy_energy("Bob", "Alice", 20)  # Bob buys 20 units of Alice's energy

# Note: In a real application, interactions with the blockchain would be handled using Web3.py to deploy and interact with smart contracts.
