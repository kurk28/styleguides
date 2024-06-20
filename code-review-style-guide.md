# Code review guidelines

This documentation contains basic information about code review best practices.

**Remember**: if a reviewer makes it very difficult for any change to go in, then developers are disincentivized to make improvements in the future.

A key point is that there is no such thing as “perfect” code—there is only better code. Reviewers should not require the author to polish every tiny piece of a MR before granting approval. Rather, the reviewer should balance out the need to make forward progress compared to the importance of the changes they are suggesting. Instead of seeking perfection, what a reviewer should seek is continuous improvement. A MR that, as a whole, improves the maintainability, readability, and understandability of the system shouldn’t be delayed for days or weeks because it isn’t “perfect.”

## Initial state while doing code-review

* Seek to understand the author’s perspective;
* Check out the branch, and test the changes locally. You can decide how much manual testing you want to perform. Your testing might result in opportunities to add automated tests;
* Technical facts and data overrule opinions and personal preferences;
* On matters of style, the style guide is the absolute authority. Any purely style point (whitespace, etc.) that is not in the style guide is a matter of personal preference. If there is no previous style, accept the author’s;
* Identify ways to simplify the code while still solving the problem;
* Offer alternative implementations, but assume the author already considered them. (“What do you think about using a custom validator here?”). Code review can have an important function of teaching developers something new about a language, a framework, or general software design principles. It’s always fine to leave comments that help a developer learn something new;
* Ensure the author is clear on what is required from them to address/resolve the suggestion:
  * Let the author know if changes are required following your review;
  * For non-mandatory suggestions, decorate with so the author knows they can optionally resolve within the merge request or follow-up at a later stage;
  * There is a difference in doing things right and doing things right now. Ideally, we should do the former, but in the real world we need the latter as well. A good example is a security fix which should be released as soon as possible. Asking the author to do the major refactoring in the merge request that is an urgent fix should be avoided;

## Main steps while doing code-review

 1. **Take a broad view of the change**: Look at the MR description and what the MR does in general;
 2. **Examine the main parts of the**: Find the file or files that are the “main” part of this MR. Often, there is one file that has the largest number of logical changes, and it’s the major piece of the MR. Look at these major parts first. This helps give context to all of the smaller parts of the MR, and generally accelerates doing the code review. If the MR is too large for you to figure out which parts are the major parts, ask the developer what you should look at first, or ask them to split up the MR into multiple MRs (see the Large MR section);
 3. **Look through the rest of the MR in an appropriate sequence**: Once you’ve confirmed there are no major design problems with the MR as a whole, try to figure out a logical sequence to look through the files while also making sure you don’t miss reviewing any file. Sometimes it’s also helpful to read the tests first before you read the main code, because then you have an idea of what the change is supposed to be doing;

## Speed of code review

If you are not in the middle of a focused task, you should do a code review shortly after it comes in. **One business day is the maximum time it should take to respond**. Following these guidelines means that a typical MR should get multiple rounds of review (if needed) within a single day.

Remember: **If you are in the middle of a focused task, such as writing code, don’t interrupt yourself to do a code review**. Instead, wait for a break point in your work before you respond to a request for review. This could be when your current coding task is completed, after lunch, returning from a meeting, coming back from the breakroom, etc.

If you are too busy to do a full review on a MR when it comes in, you can still send a quick response that lets the developer know when you will get to it, suggest other reviewers who might be able to respond more quickly.

Most deadlines are soft deadlines, not hard deadlines. They represent a desire for a feature to be done by a certain time. They are important, but you shouldn’t be sacrificing code health to make them.

## Large MR

If somebody sends you a code review that is so large you’re not sure when you will be able to have time to review it, your typical response should be to ask the developer to split the MR into several smaller MRs that build on each other, instead of one huge MR that has to be reviewed all at once. This is usually possible and very helpful to reviewers, even if it takes additional work from the developer.

If a MR can’t be broken up into smaller MRs, and you don’t have time to review the entire thing quickly, then at least write some comments on the overall design of the MR and send it back to the developer for improvement. One of your goals as a reviewer should be to always unblock the developer or enable them to take some sort of further action quickly, without sacrificing code health to do so.

## How to write code review comments

* Be kind;
* **Explain your reasoning**: the “good” example  helps the developer understand why you are making your comment. You don’t always need to include this information in your review comments, but sometimes it’s appropriate to give a bit more explanation around your intent, the best practice you’re following, or how your suggestion improves code health.
* Balance giving explicit directions with just pointing out problems and letting the developer decide;
* Encourage developers to simplify code or add code comments instead of just explaining the complexity to you;

## Resolving conflicts

When a developer disagrees with your suggestion, first take a moment to consider if they are correct. Often, they are closer to the code than you are, and so they might really have a better insight about certain aspects of it. Does their argument make sense? Does it make sense from a code health perspective? If so, let them know that they are right and let the issue drop.

However, developers are not always right. In this case the reviewer should further explain why they believe that their suggestion is correct. A good explanation demonstrates both an understanding of the developer’s reply, and additional information about why the change is being requested.

Sometimes it takes a few rounds of explaining a suggestion before it really sinks in. Just make sure to always stay polite and let the developer know that you hear what they’re saying, you just don’t agree.

In any conflict on a code review, the first step should always be for the developer and reviewer to try to come to consensus, based on the contents of this document. When coming to consensus becomes especially difficult, it can help to have a face-to-face meeting or a video conference between the reviewer and the author, instead of just trying to resolve the conflict through code review comments.

If that doesn’t resolve the situation, the most common way to resolve it would be to escalate. Often the escalation path is to a broader team discussion, having a Technical Lead weigh in, asking for a decision from a maintainer of the code, or asking an Eng Manager to help out. Don’t let a MR sit around because the author and the reviewer can’t come to an agreement.
