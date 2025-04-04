using System;
using System.Media;
using System.Threading;

class CyberSecurityChatbot
{
    static void Main()
    {
        
        PlayGreeting();

        
        DisplayAsciiArt();

        
        StartChat();
    }


    static void PlayGreeting()
    {
        try
        {
            SoundPlayer player = new SoundPlayer("greeting.ogg");
            player.PlaySync(); // Plays the sound synchronously
        }
        catch (Exception e)
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Error playing the audio file: " + e.Message);
            Console.ResetColor();
        }
    }


    static void DisplayAsciiArt()
    {
        Console.ForegroundColor = ConsoleColor.Cyan;
        Console.WriteLine(@"
   ██████╗██╗   ██╗███████╗███████╗███████╗██╗   ██╗███████╗
  ██╔════╝██║   ██║██╔════╝██╔════╝██╔════╝██║   ██║██╔════╝
  ██║     ██║   ██║█████╗  █████╗  █████╗  ██║   ██║███████╗
  ██║     ██║   ██║██╔══╝  ██╔══╝  ██╔══╝  ██║   ██║╚════██║
  ╚██████╗╚██████╔╝██║     ██║     ██║     ╚██████╔╝███████║
   ╚═════╝ ╚═════╝ ╚═╝     ╚═╝     ╚═╝      ╚═════╝ ╚══════╝
        ");
        Console.ResetColor();
    }


    static void StartChat()
    {
        Console.Write("\nHello! What's your name? ");
        string userName = Console.ReadLine();

        if (string.IsNullOrWhiteSpace(userName))
        {
            userName = "User"; // Default name if empty
        }

        Console.WriteLine($"\nWelcome, {userName}! I'm the Cybersecurity Awareness Bot.");
        Chat(userName);
    }


    static void Chat(string userName)
    {
        Console.WriteLine("\nYou can ask me about cybersecurity topics like phishing, passwords, and safe browsing.");
        Console.WriteLine("Type 'exit' anytime to end the chat.\n");

        while (true)
        {
            Console.Write("\nYou: ");
            string userInput = Console.ReadLine().ToLower();

            if (string.IsNullOrWhiteSpace(userInput))
            {
                Console.WriteLine("Bot: Please enter a valid question.");
                continue;
            }

            switch (userInput)
            {
                case "how are you?":
                    Console.WriteLine("Bot: I'm just a bot, but I'm here to help!");
                    break;
                case "what's your purpose?":
                    Console.WriteLine("Bot: I am here to help you learn about cybersecurity and stay safe online.");
                    break;
                case "what can i ask you about?":
                    Console.WriteLine("Bot: You can ask me about phishing, password safety, and safe browsing.");
                    break;
                case "what is phishing?":
                    Console.WriteLine("Bot: Phishing is a cyber attack where attackers trick you into providing personal details by pretending to be someone you trust.");
                    break;
                case "how do i create a strong password?":
                    Console.WriteLine("Bot: Use a mix of uppercase, lowercase, numbers, and symbols. Avoid personal info and common words.");
                    break;
                case "exit":
                    Console.WriteLine("Bot: Goodbye! Stay safe online.");
                    return; // Exits the chat
                default:
                    Console.WriteLine("Bot: I didn't quite understand that. Could you rephrase?");
                    break;
            }
        }
    }
}
