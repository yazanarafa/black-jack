//# black-jack
//its a game 
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <vector>
#include <string>

using namespace std;

// Function to get a random card between 1 and 11
int drawCard() {
    return rand() % 11 + 1;
}

// Function to calculate total value of cards
int calculateTotal(const vector<int>& hand) {
    int total = 0;
    int aceCount = 0;

    for (int card : hand) {
        total += card;
        if (card == 11) aceCount++;
    }

    // Adjust for Aces (11 -> 1) if over 21
    while (total > 21 && aceCount > 0) {
        total -= 10;
        aceCount--;
    }

    return total;
}

// Function to display current hand
void showHand(const vector<int>& hand, const string& owner) {
    cout << owner << "'s cards: ";
    for (int card : hand) {
        cout << card << " ";
    }
    cout << "(Total: " << calculateTotal(hand) << ")" << endl;
}

int main() {
    srand(static_cast<unsigned int>(time(0)));

    vector<int> playerHand;
    vector<int> dealerHand;

    // Initial draw
    playerHand.push_back(drawCard());
    playerHand.push_back(drawCard());
    dealerHand.push_back(drawCard());
    dealerHand.push_back(drawCard());

    cout << "=== BLACKJACK ===" << endl;
    showHand(playerHand, "Player");
    cout << "Dealer shows: " << dealerHand[0] << " [?]" << endl;

    // Player's turn
    char choice;
    while (true) {
        cout << "Do you want to (H)it or (S)tand? ";
        cin >> choice;

        if (tolower(choice) == 'h') {
            playerHand.push_back(drawCard());
            showHand(playerHand, "Player");

            if (calculateTotal(playerHand) > 21) {
                cout << "You busted! Dealer wins." << endl;
                return 0;
            }
        }
        else if (tolower(choice) == 's') {
            break;
        }
        else {
            cout << "Invalid choice. Please enter H or S." << endl;
        }
    }

    // Dealer's turn
    cout << "\nDealer's turn..." << endl;
    showHand(dealerHand, "Dealer");

    while (calculateTotal(dealerHand) < 17) {
        cout << "Dealer hits." << endl;
        dealerHand.push_back(drawCard());
        showHand(dealerHand, "Dealer");

        if (calculateTotal(dealerHand) > 21) {
            cout << "Dealer busted! You win." << endl;
            return 0;
        }
    }

    // Final result
    int playerTotal = calculateTotal(playerHand);
    int dealerTotal = calculateTotal(dealerHand);

    cout << "\n=== FINAL RESULT ===" << endl;
    showHand(playerHand, "Player");
    showHand(dealerHand, "Dealer");

    if (playerTotal > dealerTotal) {
        cout << "You win!" << endl;
    }
    else if (playerTotal < dealerTotal) {
        cout << "Dealer wins!" << endl;
    }
    else {
        cout << "It's a tie!" << endl;
    }

    return 0;
}
