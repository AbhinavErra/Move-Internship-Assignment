# Move Internship Assignment
                                            Move Internship Take Home Interview
                                            
Assignment Solutions by 
Abhinav (abhinaverra79@gmail.com) 


1.Upvote game

 A simplified version of the upvote game logic in Java. Note that the code assumes certain 
classes and methods, and you may need to adapt it to fit your specific implementation 
details.
import java.util.ArrayList;
import java.util.List;
// Define a class for the wallet
class Wallet {
 private long amount;
 private ObjectCore owner; // Assuming ObjectCore is a custom class for ownership
 public Wallet(long amount, ObjectCore owner) {
 this.amount = amount;
 this.owner = owner;
 }
 // Getter methods for amount and owner
 public long getAmount() {
 return amount;
 }
 public ObjectCore getOwner() {
 return owner;
 }
}
// Define a class for the proposal
class Proposal {
 private String name;
 private ObjectCore proposer;
 private long upvotes;
 public Proposal(String name, ObjectCore proposer) {
 this.name = name;
 this.proposer = proposer;
 this.upvotes = 0;
 }
 // Getter methods for name, proposer, and upvotes
 public String getName() {
 return name;
 }
 public ObjectCore getProposer() {
 return proposer;
 }
 public long getUpvotes() {
 return upvotes;
 }
 // Method to upvote the proposal
 public void upvote(long numUpvotes) {
 upvotes += numUpvotes;
 }
}
// Define a class for the ongoing round
class Aptos {
 private List<Proposal> proposals;
 private long maxUpvotes;
 public Aptos(long maxUpvotes) {
 this.proposals = new ArrayList<>();
 this.maxUpvotes = maxUpvotes;
 }
 // Getter methods for proposals and maxUpvotes
 public List<Proposal> getProposals() {
 return proposals;
 }
 public long getMaxUpvotes() {
 return maxUpvotes;
 }
}
// Define a class for the upvote game
public class UpvoteGame {
 private Aptos currentRound;
 private long upvotePrice;
 public UpvoteGame(long upvotePrice) {
 this.currentRound = null;
 this.upvotePrice = upvotePrice;
 }
 // Method to create a wallet in exchange for a fixed amount Y
 public Wallet getWallet(long amount) {
 ObjectCore owner = new ObjectCore(); // Assuming ObjectCore has a default 
constructor
 return new Wallet(amount, owner);
 }
 // Method to create a proposal and start a round
 public Proposal createProposal(String name, ObjectCore proposer) {
 Proposal proposal = new Proposal(name, proposer);
 // Check if there is an ongoing round, if not, start a new round
 if (currentRound == null) {
 long maxUpvotes = /* Set your desired value for MAX_UPVOTES */;
 currentRound = new Aptos(maxUpvotes);
 }
 // Add the proposal to the current round
 currentRound.getProposals().add(proposal);
 return proposal;
 }
 // Method to upvote a proposal
 public void upvoteProposal(Proposal proposal, long amount) {
 // Check if there is an ongoing round
 if (currentRound != null) {
 // Find the proposal in the current round based on the provided object
 Proposal foundProposal = currentRound.getProposals().stream()
 .filter(p -> p.equals(proposal))
 .findFirst()
 .orElse(null);
 // Check if the proposal was found and the proposer is not the same as the one 
upvoting
 if (foundProposal != null 
&& !foundProposal.getProposer().equals(proposal.getProposer())) {
 // Calculate the number of upvotes based on the provided amount and upvote 
price
 long numUpvotes = amount / upvotePrice;
 // Upvote the proposal
 foundProposal.upvote(numUpvotes);
 }
 }
 }
 // Method to finish a round and determine the winners
 public void finishRound() {
 if (currentRound != null) {
 // Determine the winning proposal based on the highest number of upvotes
 Proposal winner = currentRound.getProposals().stream()
 .max((p1, p2) -> Long.compare(p1.getUpvotes(), p2.getUpvotes()))
 .orElse(null);
 if (winner != null) {
 // Calculate the prize distribution
 long winnerPrize = currentRound.getMaxUpvotes() * upvotePrice;
 long otherPrize = winnerPrize / 2;
 long dappPrize = otherPrize * (currentRound.getProposals().size() - 1);
 // Distribute prizes
 winner.getProposer().getWallet().addAmount(winnerPrize);
 for (Proposal p : currentRound.getProposals()) {
 if (!p.equals(winner)) {
 p.getProposer().getWallet().addAmount(otherPrize);
 }
 }
 // Transfer half of the other proposals' prizes to the dapp's wallet
 // (Assuming there is a method to transfer funds between wallets)
 // dappWallet.addAmount(dappPrize);
 }
 // Reset the current round
 currentRound = null;
 }
 }
 // Main method for testing
 public static void main(String[] args) {
 // Example usage of UpvoteGame
 UpvoteGame upvoteGame = new UpvoteGame(/* Set your desired value for 
UPVOTE_PRICE */);
 // Create wallets for participants
 Wallet wallet1 = upvoteGame.getWallet(100); // Set the initial amount for wallet1
 Wallet wallet2 = upvoteGame.getWallet(150); // Set the initial amount for wallet2
 // Create proposals and start a round
 ObjectCore proposer1 = wallet1.getOwner();
 ObjectCore proposer2 = wallet2.getOwner();
 Proposal proposal1 = upvoteGame.createProposal("Proposal 1", proposer1);
 Proposal proposal2 = upvoteGame.createProposal("Proposal 2", proposer2);
 // Participants upvote proposals
 upvoteGame.upvoteProposal(proposal1, 50); // Participant 1 upvotes proposal1 with 
50 units
 upvoteGame.upvoteProposal(proposal2, 30); // Participant 2 upvotes proposal2 with 
30 units
 // Finish the round to determine winners and distribute prizes
 upvoteGame.finishRound();
 }
}


2.Ticket Market

This code assumes the existence of certain classes and methods like Aptos(with token 
transfer capability), ObjectCore, and appropriate methods for wallet interactions. 
Adjustments may be needed based on your actual implementation and requirements.
import java.util.ArrayList;
import java.util.List;
class Aptos {
 // Assume Aptos has methods for token transfer
 public void transferTokens(Wallet from, Wallet to, long amount) {
 // Implementation details for token transfer
 }
}
class ObjectCore {
 // Assuming a basic ObjectCore class
}
class Wallet {
 private long amount;
 public Wallet(long amount) {
 this.amount = amount;
 }
 public void addAmount(long additionalAmount) {
 this.amount += additionalAmount;
 }
 public void subtractAmount(long deductionAmount) {
 this.amount -= deductionAmount;
 }
 public long getAmount() {
 return amount;
 }
}
class Event {
 private String eventName;
 private long initialPrice;
 private int totalTickets;
 private long eventDate;
 private List<Ticket> tickets;
 public Event(String eventName, long initialPrice, int totalTickets, long eventDate) {
 this.eventName = eventName;
 this.initialPrice = initialPrice;
 this.totalTickets = totalTickets;
 this.eventDate = eventDate;
 this.tickets = new ArrayList<>();
 generateTickets();
 }
 private void generateTickets() {
 for (int i = 0; i < totalTickets; i++) {
 tickets.add(new Ticket(initialPrice));
 }
 }
 public String getEventName() {
 return eventName;
 }
 public long getInitialPrice() {
 return initialPrice;
 }
 public int getTotalTickets() {
 return totalTickets;
 }
 public long getEventDate() {
 return eventDate;
 }
 public List<Ticket> getTickets() {
 return tickets;
 }
}
class Ticket {
 private long price;
 public Ticket(long price) {
 this.price = price;
 }
 public long getPrice() {
 return price;
 }
}
class TicketMarket {
 private List<Event> events;
 private List<TicketListing> ticketListings;
 public TicketMarket() {
 this.events = new ArrayList<>();
 this.ticketListings = new ArrayList<>();
 }
 public void createEvent(String eventName, long initialPrice, int totalTickets, long 
eventDate) {
 Event event = new Event(eventName, initialPrice, totalTickets, eventDate);
 events.add(event);
 }
 public void sellTicket(Event event, long sellingPrice) {
 Ticket ticket = findTicket(event);
 if (ticket != null) {
 ticketListings.add(new TicketListing(event, ticket, sellingPrice));
 }
 }
 public void buyTicket(Buyer buyer, TicketListing ticketListing) {
 long buyingPrice = ticketListing.getSellingPrice();
 long commission = (long) (0.01 * buyingPrice);
 buyer.getWallet().subtractAmount(buyingPrice + commission);
 ticketListing.getSeller().getWallet().addAmount(buyingPrice - commission);
 Aptos.transferTokens(ticketListing.getTicket(), buyer.getWallet(), 1); // Assume 
transfer 1 token
 ticketListings.remove(ticketListing);
 }
 private Ticket findTicket(Event event) {
 for (Ticket ticket : event.getTickets()) {
 if (isTicketAvailable(ticket)) {
 return ticket;
 }
 }
 return null;
 }
 private boolean isTicketAvailable(Ticket ticket) {
 // Check ticket availability based on your criteria
 return true;
 }
}
class Buyer {
 private Wallet wallet;
 public Buyer(Wallet wallet) {
 this.wallet = wallet;
 }
 public Wallet getWallet() {
 return wallet;
 }
}
class Seller {
 private Wallet wallet;
 public Seller(Wallet wallet) {
 this.wallet = wallet;
 }
 public Wallet getWallet() {
 return wallet;
 }
}
class TicketListing {
 private Event event;
 private Ticket ticket;
 private long sellingPrice;
 public TicketListing(Event event, Ticket ticket, long sellingPrice) {
 this.event = event;
 this.ticket = ticket;
 this.sellingPrice = sellingPrice;
 }
 public Event getEvent() {
 return event;
 }
 public Ticket getTicket() {
 return ticket;
 }
 public long getSellingPrice() {
 return sellingPrice;
 }
 public Seller getSeller() {
 // Implement logic to find the seller based on your criteria
 return new Seller(new Wallet(0));
 }
}


3. Bank
   
Below is a Java implementation of a simple Bank simulation with the described features. 
This is a basic representation and does not include features like multi-threading or 
persistent storage, which might be important for a real-world application.
import java.util.ArrayList;
import java.util.List;
class Bank {
 private static final double INIT_CORPUS = 1000000.0; // Initial bank corpus
 private static final double MIN_DURATION = 10.0; // Minimum loan duration in minutes
 private static final double MAX_DURATION = 60.0; // Maximum loan duration in minutes
 private static final double PENALTY_RATE = 0.1; // Penalty rate for late payment
 private static final double PREPAYMENT_FEE_RATE = 0.05; // Pre-payment fee rate
 private double bankCorpus;
 private List<Loan> loans;
 public Bank() {
 this.bankCorpus = INIT_CORPUS;
 this.loans = new ArrayList<>();
 }
 public double getBankCorpus() {
 return bankCorpus;
 }
 public void applyForLoan(Customer customer, double loanAmount, double duration) {
 double interestRate = calculateInterestRate(duration);
 Loan loan = new Loan(customer, loanAmount, duration, interestRate);
 loans.add(loan);
 bankCorpus -= loanAmount; // Reduce bank corpus by the loan amount
 }
 public void processInstallment(Loan loan, double installmentAmount) {
 double remainingPrincipal = loan.getRemainingPrincipal();
 double interest = loan.calculateInterest();
 double totalPayment = interest + remainingPrincipal;
 if (installmentAmount >= totalPayment) {
 // Process pre-payment with a fee
 double prepaymentAmount = installmentAmount - totalPayment;
 double prepaymentFee = prepaymentAmount * PREPAYMENT_FEE_RATE;
 customer.pay(prepaymentFee);
 remainingPrincipal = 0.0; // Fully paid
 } else {
 // Process regular installment
 remainingPrincipal -= installmentAmount;
 }
 loan.setRemainingPrincipal(remainingPrincipal);
 bankCorpus += installmentAmount; // Increase bank corpus by the installment 
amount
 }
 public void processLatePayment(Loan loan) {
 double penalty = loan.getRemainingPrincipal() * PENALTY_RATE;
 loan.setRemainingPrincipal(loan.getRemainingPrincipal() + penalty);
 bankCorpus += penalty; // Increase bank corpus by the penalty amount
 }
 private double calculateInterestRate(double duration) {
 // Assumption: Inverse proportionality between interest rate and duration
 return 1.0 / duration;
 }
}
class Customer {
 private double wallet;
 public Customer(double wallet) {
 this.wallet = wallet;
 }
 public void pay(double amount) {
 wallet -= amount;
 }
 public double getWallet() {
 return wallet;
 }
}
class Loan {
 private Customer customer;
 private double loanAmount;
 private double duration;
 private double interestRate;
 private double remainingPrincipal;
 public Loan(Customer customer, double loanAmount, double duration, double 
interestRate) {
 this.customer = customer;
 this.loanAmount = loanAmount;
 this.duration = duration;
 this.interestRate = interestRate;
 this.remainingPrincipal = loanAmount;
 }
 public double getRemainingPrincipal() {
 return remainingPrincipal;
 }
 public void setRemainingPrincipal(double remainingPrincipal) {
 this.remainingPrincipal = remainingPrincipal;
 }
 public double calculateInterest() {
 return remainingPrincipal * interestRate;
 }
}
public class Main {
 public static void main(String[] args) {
 // Example usage of the Bank simulation
 Bank bank = new Bank();
 Customer customer = new Customer(5000.0);
 // Apply for a loan
 bank.applyForLoan(customer, 1000.0, 30.0); // Loan amount, duration in minutes
 // Simulate regular installment payment
 Loan loan = bank.getLoans().get(0);
 bank.processInstallment(loan, 300.0); // Installment amount
 // Simulate late payment
 bank.processLatePayment(loan);
 // Print updated values
 System.out.println("Bank Corpus: $" + bank.getBankCorpus());
 System.out.println("Customer Wallet: $" + customer.getWallet());
 System.out.println("Remaining Loan Principal: $" + loan.getRemainingPrincipal());
 }
}

This Java code provides a basic simulation of a bank with the described features. It includes 
a Bank class, Customer class, Loan class, and a simple Main class for example usage. Please 
note that this code is for illustrative purposes and may need further refinement based on 
your specific requirements and the actual operating environment
