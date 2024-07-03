# CODEALPHA-2
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Scanner;

class Stock {
    String symbol;
    double price;

    public Stock(String symbol, double price) {
        this.symbol = symbol;
        this.price = price;
    }
}

class Portfolio {
    HashMap<String, Integer> stocks = new HashMap<>();

    public void buyStock(String symbol, int quantity) {
        stocks.put(symbol, stocks.getOrDefault(symbol, 0) + quantity);
    }

    public void sellStock(String symbol, int quantity) {
        if (stocks.containsKey(symbol) && stocks.get(symbol) >= quantity) {
            stocks.put(symbol, stocks.get(symbol) - quantity);
            if (stocks.get(symbol) == 0) {
                stocks.remove(symbol);
            }
        } else {
            System.out.println("Not enough stocks to sell.");
        }
    }

    public void displayPortfolio(HashMap<String, Stock> marketData) {
        double totalValue = 0;
        System.out.println("Your Portfolio:");
        for (String symbol : stocks.keySet()) {
            int quantity = stocks.get(symbol);
            double price = marketData.get(symbol).price;
            double value = price * quantity;
            totalValue += value;
            System.out.println(symbol + ": " + quantity + " shares, Value: $" + value);
        }
        System.out.println("Total Portfolio Value: $" + totalValue);
    }
}

public class StockTradingPlatform {
    static HashMap<String, Stock> marketData = new HashMap<>();
    static Portfolio portfolio = new Portfolio();

    public static void main(String[] args) {
        initializeMarketData();
        Scanner scanner = new Scanner(System.in);
        String command;

        while (true) {
            System.out.println("\nEnter a command (buy/sell/display/exit): ");
            command = scanner.nextLine();

            switch (command.toLowerCase()) {
                case "buy":
                    System.out.println("Enter stock symbol: ");
                    String buySymbol = scanner.nextLine();
                    System.out.println("Enter quantity: ");
                    int buyQuantity = scanner.nextInt();
                    scanner.nextLine();  // consume newline
                    buyStock(buySymbol, buyQuantity);
                    break;

                case "sell":
                    System.out.println("Enter stock symbol: ");
                    String sellSymbol = scanner.nextLine();
                    System.out.println("Enter quantity: ");
                    int sellQuantity = scanner.nextInt();
                    scanner.nextLine();  // consume newline
                    sellStock(sellSymbol, sellQuantity);
                    break;

                case "display":
                    portfolio.displayPortfolio(marketData);
                    break;

                case "exit":
                    System.out.println("Exiting the platform.");
                    scanner.close();
                    System.exit(0);

                default:
                    System.out.println("Invalid command. Try again.");
                    break;
            }
        }
    }

    public static void initializeMarketData() {
        marketData.put("AAPL", new Stock("AAPL", 150.00));
        marketData.put("GOOGL", new Stock("GOOGL", 2800.00));
        marketData.put("AMZN", new Stock("AMZN", 3400.00));
        marketData.put("MSFT", new Stock("MSFT", 299.00));
        marketData.put("TSLA", new Stock("TSLA", 730.00));
    }

    public static void buyStock(String symbol, int quantity) {
        if (marketData.containsKey(symbol)) {
            portfolio.buyStock(symbol, quantity);
            System.out.println("Bought " + quantity + " shares of " + symbol);
        } else {
            System.out.println("Stock symbol not found.");
        }
    }

    public static void sellStock(String symbol, int quantity) {
        if (marketData.containsKey(symbol)) {
            portfolio.sellStock(symbol, quantity);
            System.out.println("Sold " + quantity + " shares of " + symbol);
        } else {
            System.out.println("Stock symbol not found.");
        }
    }
}
