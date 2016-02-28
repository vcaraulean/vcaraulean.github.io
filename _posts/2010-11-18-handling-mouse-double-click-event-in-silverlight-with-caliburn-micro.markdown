---
comments: true
date: 2010-11-18 11:32:03
layout: post
title: Handling Mouse Double Click event in Silverlight with Caliburn.Micro
categories: [silverlight]
---

There are times when you want to handle a “MouseDoubleClick” event in your code and execute something in your ViewModel as a response to the event . Back in the day, I’ve done few implementations of mechanism that will allow do this, for both WPF and Silverlight platforms, with different degrees of success and (developer) satisfaction. The one I’ll be talking is, so far, the prettiest and I like how it works. Mostly because of using [Actions from Caliburn.Micro](http://caliburnmicro.codeplex.com/wikipage?title=All%20About%20Actions&referringTitle=Documentation) framework. The Actions mechanics gives a very neat way to execute logic in VM’s as a response to UI events. A typical example for declaring an action will be:
 
    <Button Content="Remove"
            cal:Message.Attach="[Event Click] = [Action Remove($dataContext)]" />

Back to the Mouse Double Click or, better said, to lack of such event in Silverlight. 

Most of implementations you’ll find in internet will play with catching sequential clicks or “MouseLeftButtonUp” events and then deciding, it was a double click or not (using TimeSpan and distance between clicks, [sample implementation](http://www.michaelsnow.com/2010/05/10/silverlight-tip-of-the-day-17-double-click/)). The interesting bit however was to link this event to an Action to be executed by Caliburn.Micro on your ViewModel and reuse that for any visual elements.

First, I’ve set the API how I want to see it:

    <TextBlock Text="{Binding DisplayName}"
               Behaviors:DoubleClickEvent.AttachAction="Open($datacontext)" />

It will be a behavior that can be attached to any UI Element. In Action string we’re skipping specification of what event will be the Action attached to, because we are emulating one event – DoubleClick.

Attaching of the Action string to MouseLeftButtonUp is the tricky part. Mostly because to achieve that I had to modify declaration of one method in Caliburn.Micro for making it public. See [this changeset](https://hg01.codeplex.com/forks/vcaraulean/caliburnmicro/rev/ab06fc1697c7) in my CM fork if you’re interested in details, but I’ll be asking CM’s authors to incorporate the change in main repository.

Attaching action string:

    private static void AttachActionToTarget(string text, DependencyObject d)
    {
        var actionMessage = Parser.CreateMessage(d, text);

        var trigger = new ConditionalEventTrigger
        {
            EventName = "MouseLeftButtonUp",
            Condition = e => DoubleClickCatcher.IsDoubleClick(d, e)
        };
        trigger.Actions.Add(actionMessage);

        Interaction.GetTriggers(d).Add(trigger);
    }


ConditionalEventTrigger is a small class that wraps EventTrigger and adding a condition that will be checked before raising event. If it’s false, event will not be raised. Snippet:

    public class ConditionalEventTrigger : EventTrigger
    {
        public Func<EventArgs, bool> Condition { get; set; }

        protected override void OnEvent(EventArgs eventArgs)
        {
            if (Condition(eventArgs))
                base.OnEvent(eventArgs);
        }
    }

I’ll not show here DoubleClickCatcher class because it’s not small, but you can find all related classes in [this gist](https://gist.github.com/704830) on GitHub.

So far, I’m satisfied by this solution. Neat, simple and it works like a charm for us. I’ve used it with ListBox and DataGrid controls, but I think it should work on pretty any other visual element.

**[Complete source code](https://gist.github.com/704830).** Use with care. Any suggestions, remarks or bug reports are welcome.
