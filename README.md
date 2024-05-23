using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;


namespace MinCalculator
{
    class Program
    {
        static void Main(string[] args)
        {
            bool continueCalculating = true;

            while (continueCalculating)
            {
                try 
                {
                    double firstNumber = GetNumber("Skriv ditt första nummer");           //Här får man första nummret av användaren
                    double secondNumber = GetNumber("Skriv ditt andra nummer");           //Här får man andra nummre av använderen
                    Console.WriteLine("Vilket räknesätt vill du använda (+, -, /, *): "); //För att få Räknesättet av användaren
                    string myOperator = Console.ReadLine();
                    double result = Calculate(firstNumber, secondNumber, myOperator);     //Gör uträkningen baserat på valen av användaren
                    Console.WriteLine($"Resultat: {result}");                             //Visar resultatet
                }
                catch (Exception ex)
                {
                    Console.WriteLine($"Fel: {ex.Message}");                              //Hanterar eventuella fel med inmatningen och visar ett meddelande, "fel"
                }
                Console.WriteLine("Vill du göra en ny beräkning? (ja/nej): ");            //Frågar om användaren vill göra en ny beräkning
                string response = Console.ReadLine().ToLower();

                if (response != "ja")                                                     //Om svaret inte är ja, avslutas While loopen
                {
                    continueCalculating = false;
                }
            }
        }
        static double GetNumber(string prompt)                                            //Metod för att kolla om det inmatade nummret är giltigt med felhantering
        {
            double number;
            while (true)
            {
                Console.WriteLine(prompt);
                if (double.TryParse(Console.ReadLine(), out number))                      //Kollar om användarens inmatning kan bli ett nummer
                {
                    return number;                                                        //Om det kan ses som ett nummer skickas det tillbaka
                }
                else
                {
                    Console.WriteLine("Ogiltig info, försök igen. ");                     //Om det inte är giltigt så skickar den ett felmeddelande
                }
            }
        }
        static double Calculate(double firstNumber, double secondNumber, string myOperator)  //Denna används för att göra beräkningar för de olika räknesätten
        {
            return myOperator switch                                                      //Ett "switch" uttryck för att kunna hantera de olika räknesätten
            {
                "+" => firstNumber + secondNumber,                                        //Addition
                "-" => firstNumber - secondNumber,                                        //Subtration
                "/" when secondNumber != 0 => firstNumber / secondNumber,                 //Division som kollar så att det inte delas med 0
                "/" => throw new DivideByZeroException("Du kan inte dela med 0 dummer"),  //Delas något med 0 skickar den ett felmeddelande
                "*" => firstNumber * secondNumber,                                        //Multiplikation
                _ => throw new InvalidOperationException("Du valde ingen av de möjliga räknesätten")  //Skickar ett felmeddelande om man inte väljer ett räknesätt
            };
        }
    }
}
