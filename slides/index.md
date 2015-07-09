- title : TalBot
- description : Introduction to TalBot
- author : Mike Talbott
- theme : blood
- transition : default

***
- data-background : images/hal3.jpg

***
- data-background : images/hal3.jpg

### TalBot
#### Slacking on F#

' Hello. This will be a presentation about my experiences learning F# while building TalBot, a slack bot.
' I'll make my slides available after the presentation and will post a message in slack with a link when they are ready.
  
***

### Why TalBot
- Level up
- Provide value
- Have an adventure

' TalBot has been a tool for leveling up my skills.
' It is a simple project to experiment with new concepts and different ways of thinking.
' Through it I've learned more than just functional programming and F#.
' I've also experienced and learned about several other technologies and concepts that were not part of my original agenda.
' One of my goals has been for TalBot to be something that provides value to the team via status notifications & jira links
' I'm also hoping to spark some contributions that will provide other useful or just fun features.
' So far the experience has been an adventure.
' Azure websites, Azure Service Bus, Designing Distributed systems, Web Sockets, OAuth, the Actor model, Asynchronous operations, and Windows Services.

***

### Why Functional

- Uncle Bob
  - [The Failure of State.](https://vimeo.com/97514630)
- Scott Wlaschin 
  - [Domain modeling with the F# type system](https://vimeo.com/channels/c4fsharp/97507575)
  - [Railway Oriented Programming](https://vimeo.com/97344498)
  
' There's a case to be made for the advantages of a functional programming style. I won't specifically address that today.
' But, if you're interested, here are a few good presentations.

***

### Why F#

- Reduces time to market
- Correctness (fewer bugs)
- Simplifies complex problems
- Scales well

> Tomas Petricek - [Making the case for F#](https://channel9.msdn.com/Blogs/Seth-Juarez/Making-the-Case-for-using-F-with-Tomas-Petricek)

' As with functional programming, others make the case better than I could about why to use F#.
' These are a few of the characteristics that are used to promote the language.
' These characteristics aren't exclusive to F# but they are some of it's strengths.
' I won't go into detail now, but Tomas can explain more or we can talk later if you're interested.
' -REPL (Read - eval - print loop) Interactive prototyping that can be turned into unit tests, type providers
' -Correct fewer bugs You can use the domain model to ensure valid state (discriminated unions, pattern matching, units of measure, absence of null)
' -Manage complexity to handle more complex problems (type inference - similar to how var infers local variables, but not limited to the local scope, immutability)
' -Scales well (concurrency & parallelism)

***

### Why I tried F#

- Hybrid language
- .Net interoperability
- To write better C#
' I chose to learn F# because I felt it would have a lower barrier to entry than a different functional language on an unfamiliar stack.
' It is a hybrid language that is functional first but also supports an object oriented approach. It also includes .Net interoperability, 
' Most of all I had heard that learning F# would help me write better C# on a day to day basis.

***

### What I like about F#

- Community
- Syntax
- Composability
' One great thing about F# is the community. There's great support on slack, twitter, etc.
' After getting past the initial unfamiliarity the syntax is simple.
' After a bit the language starts to feel natural and just flow.
' I think this is largely because of the ability to easily break functions into smaller pieces and to pass functions around.

***

### Challenges

![](images/Monitor-Throw.gif)
' There have been some challenges and moments of frustration along the way.

---

### F# Examples

' When I first started learning, the concepts were exciting but the examples weren't clicking for me.
' Many were not relatable and too mathematics focused.
' I found myself hung up on wanting to understand how the concepts applied to the world where we work.
' Before long, I had to divert my attention from just studying and needed to find something simple and tangible build.

---

### Privacy

![](images/privacy.gif)
' Another challenge was how to simultaneously make the project 
' - Open source where I could get community feedback and potentially contributions
' - Yet practical to implement in our environment without compromising private information.
' These challenges were addressed by implementing a plugin model to enable extended functionality specific to work.

---

### GitHub Anxiety
' A personal challenge to overcome was social anxiety related to sharing my code publicly.
' However, I realised that, if I wanted to get input from the F# community the code had to be open source and publicly available.

***

### Fails

![](images/fail.gif)
' Part of the anxiety comes from the fear of failure and particularly public failure.
' So in the spirit of facing fears and openness, here are a few of my fails...

---

### Starting
' One implementation of the bot runs as a windows service. 
' I learned the hard way that having a long running process in the OnStart method of a windows service prevents the service from transitioning out of a starting state.
' It is obvious in hindsight, but was an issue I had to sleep on before it clicked.

---

### Recursion

![](images/recursion.gif)
' Bots that talk to themselves NEVER SHUT UP. Thankfully Slack throttles API access if post frequency exceeds a threshold.
' I learned this one while testing one of my early implementations.

---

### Spam

' Some fails are less dramatic and you don't experience them until you have had some success. 
' For example figuring out how to get the bot to listen to messages without being called by keyword was a mini success.
' However, it also resulted in TalBot being overly chatty and annoying to some users.

***

### Wins
' Thankfully it's not all fails. Here are some of the wins that make it worthwhile.

---

### Community

![Great community](images/helpful.gif)
' Pushing past some of the social anxiety and finding a helpful community has been a win.
' By making my work public, asking questions on the F# slack channel, and logging issues on other open source projects I've found some great help.
' The F# community is enthusiastic about the language and welcoming to new recruits.
' In one case I logged an issue against another F# project that is a dependency for TalBot.
' The project owner helped even though the issue was actually a mistake in my project.

---

### Satisfaction

' Satisfaction of seeing results and learning along the way is another win.
' We all experience this one when we look back at something we've built or a challenge we've overcome.
' It's part of why I love what we do.

---

### Approval

![Great community](images/approval.gif)
' Of course it's all the more satisfying to receive acknowledgement from peers and the people you're building for.

***

### Plugins
[TalBotPlugins](http://petyr.harriscomputer.com/users/h80204/repos/talbotplugins/browse) on Stash
' For those that might be interested in using or contributing to TalBot
' There are a few options.
' Plugins are for functionality that doesn't belong in the Open Source repo. Typically this would be for something that's work specific.
' Current examples are the notifications of SmartFusion releases, and notifications of Sync Agent status for MGH and ESS.
' Plugins are your assemblies that implement the TalBot interface (available via NuGet).
' There are two plugin interfaces. One for notifications and one for message responses.
' There are some existing plugins in the repo on stash. HLG R&D users should have access to clone and submit pull requests.

---

#### C#

    [lang=cs]
	public class SamplePlugin : INotificationPlugin
    {
        public IEnumerable<OutgoingMessage> Run()
        {
            var message = new OutgoingMessage
				{
					Destination = "#general",
					Sender = "TalBot",
					Text = "Hello from C# sample.",
					Icon = ":c3po:"
				);
            return new OutgoingMessage[] { message };
        }
    }

' Here is a C# hello world example of a the INotificationPlugin interface.
' It is a single method that when called returns a collection of outgoing messages.

---

#### F#

	type SamplePlugin() =
		interface INotificationPlugin with
			member x.Run(): IEnumerable<OutgoingMessage> = 
				{
					OutgoingMessage.destination="#general";
					sender="TalBot";
					text="Hello from #F sample.";
					icon=":c3po:";
				}
				
' Here is an F# example of the same thing.
	
***

### Library
- High level - Bots
- Granular - Bot components
' Another option is to create your own scripts or applications using the TalBot library available via NuGet.
' There are two levels of abstraction. Bots and Bot components.

---

### F# bot
	open TalBot
	open Configuration

    let slackConfig = Configuration.slackConfiguration ()
    let bot = Bot(slackConfig)
	
	let botNotifier () = async {
		while true do
			bot.Speak ()
			do! Async.Sleep 3600000
	}
    
	Async.Start(botNotifier (), cancellationSource.Token)

' This is a sample of instantiating a bot and calling the Speak member which runs any available notification plugins.
' There are additional bot methods for doing things like listening to webhooks from slack, posting to and reading from a service bus queue, listening to slack with webhooks, responding to incoming messages, and logging exceptions to a slack channel.
	
---

### F# component
	open TalBot
	open Configuration
	open BotHelper

	let notifications = Notifier.getNotifications ()
	let groupedNotifications = notifications |> List.groupBy (fun x -> x.sender)

	let postAndLog (sender,notifications) = 
		let postNewMessagesToSlack = postNewMessages Configuration.SlackUri
		postNewMessagesToSlack (sender,notifications)
		saveMessagesToLog (sender,notifications)

	groupedNotifications |> List.iter postAndLog

' This is an example of a bot component. This level of abstraction provides more granular control. A bot is made up of multiple of these components, but the components can be used on their own.

***

### TalBot
#### you can...

- Clone it [GitHub](https://github.com/talbottmike/TalBot)
- Learn about it [GitHub.io](http://talbottmike.github.io/TalBot/)
- Use it [NuGet](https://www.nuget.org/packages/TalBot)

' Finally you can use TalBot by cloning the repo, tweaking it and deploying your own flavor.

***

#### ... one last thing.

' One last challenge and another thing you get to practice when working on side projects is deciding what to prioritize. 
' Recently that meant addressing TalBot's chattyness and not focusing on updating the documentation.
' So the documentation resources available today need some updating.
' Thanks for listening and pull requests are welcome.



