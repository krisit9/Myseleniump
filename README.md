using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using System;
using System.Collections.Generic;

class MatchingEngineTest
{
    static void Main()
    {
        IWebDriver driver = new ChromeDriver();
        driver.Navigate().GoToUrl("https://www.matchingengine.com/");

        // Expand 'Modules' in the header
        IWebElement modulesMenu = driver.FindElement(By.LinkText("Modules"));
        modulesMenu.Click();

        // Click 'Repertoire Management Module'
        IWebElement repertoireLink = driver.FindElement(By.LinkText("Repertoire Management Module"));
        repertoireLink.Click();

        // Scroll to 'Additional Features' section
        IJavaScriptExecutor js = (IJavaScriptExecutor)driver;
        js.ExecuteScript("window.scrollBy(0, 1000);"); // Adjust scroll as needed

        // Click 'Products Supported'
        IWebElement productsSupported = driver.FindElement(By.PartialLinkText("Products Supported"));
        productsSupported.Click();

        // Assert on the list of supported products
        IReadOnlyCollection<IWebElement> productItems = driver.FindElements(By.XPath("//h3[contains(text(),'There are several types of Product Supported:')]/following-sibling::ul/li"));

        Console.WriteLine("Supported Products:");
        foreach (var item in productItems)
        {
            Console.WriteLine("- " + item.Text);
        }

        // Example assertion (you can use a testing framework like NUnit for real tests)
        if (productItems.Count == 0)
        {
            Console.WriteLine("Assertion Failed: No supported products listed.");
        }
        else
        {
            Console.WriteLine("Assertion Passed: Supported products found.");
        }

        driver.Quit();
    }
}
