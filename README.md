using System;
using System.Collections.Generic;
using System.Media;

class CyberSecurityChatbot
{
    static Dictionary<string, List<string>> keywordResponses = new Dictionary<string, List<string>>()
    {
        { "password", new List<string> {
            "Use strong, unique passwords for each account.",
            "Avoid personal details in your passwords.",
            "Consider using a password manager to store your passwords securely."
        }},
        { "phishing", new List<string> {
            "Be cautious of emails asking for personal information.",
            "Always check the sender's email address for authenticity.",
            "Don't click links in suspicious messages; go directly to the site instead."
        }},
        { "scam", new List<string> {
            "Ignore messages asking for urgent money transfers.",
            "Watch out for fake job offers or investment scams.",
            "Never share your OTP or login credentials."
        }},
        { "privacy", new List<string> {
            "Adjust your privacy settings on social media.",
            "Avoid sharing sensitive information publicly online.",
            "Use end-to-end encrypted messaging apps."
        }}
    };

    static Dictionary<string, string> sentiments = new Dictionary<string, string>()
    {
        { "worried", "It's completely understandable to feel that way. Let's go over some ways to stay safe." },
        { "curious", "Curiosity is great! Here's something you might find interesting." },
        { "frustrated", "Take a deep breath — cybersecurity can be tricky, but I'm here to help." }
    };

    static string userName = "User";
    static string favouriteTopic = "";
    static string lastTopic = "";
    static Random rand = new Random();

    delegate void ResponseHandler(string input);

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
            SoundPlayer player = new SoundPlayer("greeting.wav");
            player.PlaySync();
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
        userName = Console.ReadLine();

        if (string.IsNullOrWhiteSpace(userName))
            userName = "User";

        Console.WriteLine($"\nWelcome, {userName}! I'm your Cybersecurity Awareness Bot.");
        Console.WriteLine("You can ask me about phishing, password safety, scams, and privacy.");
        Console.WriteLine("Type 'exit' anytime to end the chat.\n");

        Chat();
    }

    static void Chat()
    {
        ResponseHandler handler = ProcessInput;

        while (true)
        {
            Console.Write("\nYou: ");
            string userInput = Console.ReadLine().ToLower();

            if (string.IsNullOrWhiteSpace(userInput))
            {
                Console.WriteLine("Bot: Please enter a valid question.");
                continue;
            }

            if (userInput == "exit")
            {
                Console.WriteLine("Bot: Goodbye! Stay safe online.");
                break;
            }

            handler(userInput);
        }
    }

    static void ProcessInput(string input)
    {
        // Check for sentiment
        foreach (var sentiment in sentiments)
        {
            if (input.Contains(sentiment.Key))
            {
                Console.WriteLine($"Bot: {sentiment.Value}");
                return;
            }
        }

        // Check for topic keywords
        foreach (var topic in keywordResponses)
        {
            if (input.Contains(topic.Key))
            {
                lastTopic = topic.Key;
                favouriteTopic = topic.Key;
                string response = topic.Value[rand.Next(topic.Value.Count)];
                Console.WriteLine($"Bot: {response}");
                return;
            }
        }

        // Follow-up questions
        if (input.Contains("more") && !string.IsNullOrEmpty(lastTopic))
        {
            string response = keywordResponses[lastTopic][rand.Next(keywordResponses[lastTopic].Count)];
            Console.WriteLine($"Bot: Here's more on {lastTopic}: {response}");
            return;
        }

        // User interest memory
        if (input.Contains("interested in"))
        {
            foreach (var topic in keywordResponses.Keys)
            {
                if (input.Contains(topic))
                {
                    favouriteTopic = topic;
                    Console.WriteLine($"Bot: Great! I'll remember you're interested in {topic}. It's a very important topic.");
                    return;
                }
            }
        }

        // Recall user's interest
        if (input.Contains("what do you remember"))
        {
            if (!string.IsNullOrEmpty(favouriteTopic))
            {
                Console.WriteLine($"Bot: You mentioned you're interested in {favouriteTopic}. Here's a tip: {keywordResponses[favouriteTopic][0]}");
            }
            else
            {
                Console.WriteLine("Bot: I don't remember a specific topic yet. Let me know what you're interested in.");
            }
            return;
        }

        // Default fallback
        Console.WriteLine("Bot: I'm not sure I understand. Can you try rephrasing?");
    }
}
