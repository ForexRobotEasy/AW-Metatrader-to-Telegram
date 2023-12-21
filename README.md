//+------------------------------------------------------------------+
//|                              AW Metatrader to Telegram           |
//|                  https://www.forexroboteasy.com                  |
//|                        Developed by Forex Robot Easy Team         |
//+------------------------------------------------------------------+

// This code is an example implementation of a Metatrader Expert Advisor that sends notifications to a Telegram chat using the AW Metatrader to Telegram service.
// The code includes necessary libraries and defines the Telegram bot token and chat ID for sending messages.
// It also includes custom functions for sending messages and formatting notification messages with emojis.
// The code further includes trading functions to handle trade opening, closing, modification, and deletion.
// The OnTick function is the entry point of the Expert Advisor where the trading algorithm can be implemented.
// The code also initializes the Telegram connector and provides a link to detailed reviews and trading results of the product.

// Include necessary libraries
#include <TelegramConnector.mqh>

// Define Telegram bot token and chat ID
#define TELEGRAM_TOKEN 'YOUR_TELEGRAM_BOT_TOKEN'
#define TELEGRAM_CHAT_ID 'YOUR_TELEGRAM_CHAT_ID'

//+------------------------------------------------------------------+
//|                           Custom Functions                        |
//+------------------------------------------------------------------+

// Function to send a message to Telegram
void SendTelegramMessage(string message)
{
   CTelegramConnector telegram;
   telegram.Connect(TELEGRAM_TOKEN);
   telegram.SendMessage(TELEGRAM_CHAT_ID, message);
   telegram.Disconnect();
}

// Function to format notification message with emojis
string FormatNotification(string type, string symbol, double price)
{
   string emoji = '';
   if (type == 'open') emoji = '📈';
   if (type == 'close') emoji = '📉';
   if (type == 'modify') emoji = '✏️';
   if (type == 'delete') emoji = '❌';

   string message = 'Trade ' + emoji + '\n';
   message += 'Symbol: ' + symbol + '\n';
   message += 'Price: ' + DoubleToString(price, _Digits) + '\n';

   return message;
}

//+------------------------------------------------------------------+
//|                        Trading Functions                          |
//+------------------------------------------------------------------+

// Function to handle trade opening
void OnTradeTransaction(const MqlTradeTransaction& transaction)
{
   if (transaction.type == TRADE_TRANSACTION_ORDER_ADD)
   {
      string message = FormatNotification('open', Symbol(), transaction.price);
      SendTelegramMessage(message);
   }
}

// Function to handle trade closing
void OnTradeTransaction(const MqlTradeTransaction& transaction)
{
   if (transaction.type == TRADE_TRANSACTION_ORDER_CLOSE)
   {
      string message = FormatNotification('close', Symbol(), transaction.price);
      SendTelegramMessage(message);
   }
}

// Function to handle trade modification
void OnTradeTransaction(const MqlTradeTransaction& transaction)
{
   if (transaction.type == TRADE_TRANSACTION_ORDER_MODIFY)
   {
      string message = FormatNotification('modify', Symbol(), transaction.price);
      SendTelegramMessage(message);
   }
}

// Function to handle trade deletion
void OnTradeTransaction(const MqlTradeTransaction& transaction)
{
   if (transaction.type == TRADE_TRANSACTION_ORDER_DELETE)
   {
      string message = FormatNotification('delete', Symbol(), transaction.price);
      SendTelegramMessage(message);
   }
}

//+------------------------------------------------------------------+
//|                           Initialization                         |
//+------------------------------------------------------------------+

// Initialize the Telegram connector
CTelegramConnector telegram;

//+------------------------------------------------------------------+
//|                           Expert Advisor                          |
//+------------------------------------------------------------------+

// Entry point of the Expert Advisor
void OnTick()
{
   // Your trading algorithm goes here
}

//+------------------------------------------------------------------+

// For detailed reviews and trading results of this product, visit the following link:
// https://forexroboteasy.com/forex-robot-review/aw-metatrader-to-telegram-review-enhance-forex-trading-with-automated-notifications/

// This code is provided as a sample and is not the official product of ForexRobotEasy.
// ForexRobotEasy only showcases this code as an example of how the product can work.
// To find the official developer of this product, please refer to MQL5.
