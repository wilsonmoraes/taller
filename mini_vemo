class User:
    def __init__(self, name, balance=0, credit_card=0):
        self.name = name
        self.balance = balance
        self.credit_card = credit_card
        self.friends = set()
        self.activity = []

    def pay(self, other, amount, description):
        if self.balance >= amount:
            self.balance -= amount
            payment_method = "balance"
        elif self.credit_card >= amount:
            self.credit_card -= amount
            payment_method = "credit card"
        else:
            raise ValueError("Insufficient funds for payment")

        # record for both users
        activity_str = f"{self.name} paid {other.name} ${amount:.2f} for {description} using ({payment_method})"
        self.activity.append(activity_str)
        other.activity.append(activity_str)

    def add_friend(self, other):
        self.friends.add(other.name)
        other.friends.add(self.name)

    def retrieve_activity(self):
        return self.activity


class MiniVenmo:
    def __init__(self):
        self.users = {}

    def create_user(self, name, balance=0, credit_card=0):
        user = User(name, balance, credit_card)
        self.users[name] = user
        return user

    def render_feed(self):
        feed = []
        for user in self.users.values():
            for activity in user.activity:
                feed.append(activity)

        return sorted(list(set(feed)))

    def render_feed_with_friends(self, user_name):
        user = self.users[user_name]
        feed = []
        for friend_name in user.friends:
            friend = self.users[friend_name]
            feed.extend(friend.activity)
        feed.extend(user.activity)
        return sorted(list(set(feed)))


venmo = MiniVenmo()
bobby = venmo.create_user("Bobby", balance=10, credit_card=50)

carol = venmo.create_user("Carol", balance=5, credit_card=30)

bobby.pay(carol, 5, "Coffee")
carol.pay(bobby, 15, "Lunch")

bobby.add_friend(carol)

print(venmo.render_feed())
print(venmo.render_feed_with_friends("Bobby"))


# basic unt test, without pytest or any other framework

def test_payment():
    user1 = User("A", balance=10, credit_card=10)
    user2 = User("B", balance=0, credit_card=0)
    user1.pay(user2, 5, "Test")
    assert user1.balance == 5

def test_add_friend():
    user1 = User("A", balance=10, credit_card=10)
    user2 = User("B", balance=0, credit_card=0)
    user1.add_friend(user2)
    assert user2.name in user1.friends
    assert "B" in user1.friends


test_payment()
test_add_friend()
