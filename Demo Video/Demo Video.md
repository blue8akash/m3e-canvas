The tool shown in the video is **M3E Canvas**, a free, browser-only design tool used for sketching mobile application interfaces and generating precise natural-language prompts for AI coding agents. Rather than generating code directly, it produces a detailed textual brief that you can paste into tools like Gemini CLI, Cursor, or Claude Code to build the actual application.

## Design and Component Library

* **Material 3 Expressive Components:** The editor features a drag-and-drop interface populated with genuine Material 3 Expressive (M3E) components, including buttons, navigation bars, floating action buttons (FABs), cards, search bars, and snackbars.
* **Magnetic Connections:** Components mimic real M3E behavior, such as buttons naturally fusing into a pill group with softened facing corners when dragged close to one another.
* **Annotations:** You can attach specific behavioral notes to parts as you place them (e.g., "this opens the filter sheet"), which are carried over verbatim into the final exported brief.

## Prototyping and Navigation

* **Interactive Wiring:** Any tappable component can be linked to another screen or set to a "back" action.
* **Transitions and Swipes:** Screen connections support directional slide, fade, and expand transitions, as well as native swipe gestures.
* **Live Preview:** You can press 'P' to test the tap-through flow of your linked screens directly in the browser before exporting any instructions.

## Theming and Customization

* **Dynamic Color Palettes:** The theme panel allows you to use presets or a single seed color that automatically expands into a full Material 3 role palette, complete with light/dark modes and adjustable contrast levels.
* **Global Styling:** You can globally adjust corner shapes (square, rounded, or full) and typography (such as Roboto variants or system fonts) across the entire design simultaneously.
* **Motion Settings:** The tool includes motion configuration, such as an "expressive spring" setting that drives the animation feel during the live preview.

## Workflow and Exporting

* **AI Prompt Generation:** With one click, the tool collapses a designed screen into a structured natural-language brief (available in English or Japanese) intended for an AI coding assistant.
* **PNG Export:** Individual screens can also be saved as static PNG images if a visual mockup is required.
* **Local Storage Setup:** M3E Canvas operates entirely without a backend; there are no accounts or syncing, and all designs live directly in your browser's local storage.

Are you planning to use M3E Canvas to map out a specific application, or are you just exploring new UI design tools?

[M3E Canvas Hands-On](https://www.youtube.com/watch?v=W9-OIPM3EI4&utm_source=gemini)
This video provides a practical walkthrough on navigating the canvas, customizing themes, and exporting your final interface design as an AI prompt.

## Video Transcript 
Transcript
Search in video
Design an app without writing code
0:00
Hello everybody and welcome back to the
0:01
channel. What if I told you that there's
0:02
a tool that you could use to visualize
0:04
and design your dream app in seconds.
0:07
You don't need to hire a UI UIX person.
0:10
You don't need to hire a developer. You
0:11
can just design your app from the
0:14
comfort of your home and get it to what
0:16
you want it to look like. And not to say
0:19
you don't need to hire those guys. If
0:20
you want to, you can go down that route.
0:22
But this essentially gives you the power
0:24
to create your applications yourself
0:26
without writing a line of code. You can
0:28
design it. you can visualize it and you
0:31
can just have a go at it. And this is
0:33
not to say it's not for everyone because
0:35
even as a UX UI designer, if you are a
0:38
product owner, if you're a developer, if
0:40
you are somebody that likes to tinker
0:41
and get into the nitty-gritty things of
0:43
how you want your dream application to
0:46
look like, this one is for you. But
0:48
before we get into it, welcome to the
0:50
average tech channel. My name is Toby
0:52
and today we're going to be taking a
0:54
look at a tool I call M3 canvas. It's
0:57
not really what I call it, but that's
0:59
what it's called. Anyway, let's get
1:00
right into it. First thing you want to
Open M3E Canvas
1:02
do is head over to the
1:03
inkai.ub.io/m3-canvas
1:08
URL. I know it's a mouthful. I'm going
1:10
to have that URL placed in the
1:12
description of the video. So, please
1:13
head down there and check it out. So,
1:15
when you land on the application, you're
1:18
not going to be met with a landing page.
1:19
You're just going to be met with this
1:21
very clean, beautiful user interface.
1:24
And what you see first of all is a
1:26
mobile phone screen. So think about it
1:28
like your Android, your iPhone, whatever
1:29
it is. This is what it will look like.
1:32
There's also a desktop mode here where
1:34
you can take a look at what your app
1:35
will look like in maybe an iPad or even
1:38
a screen which maybe like a like a
1:41
desktop or laptop screen. But we're just
1:43
going to focus on mobile because that is
1:44
the in thing. Now let's go back to
Navigate the canvas and add screens
1:46
mobile. And like I said, it is a very
1:48
simple intuitive interface. What you
1:51
have is the screen. You have some
1:52
controls at the top of the screen, which
1:54
is your cursor to basically point and
1:58
drag whatever components you want out of
2:00
the lefth hand menu, but we'll get to
2:02
that in a second. You have the hand
2:04
here, which you can use to move or pan,
2:06
just move around your screen. You have
2:09
uh basically your plus or add screen
2:11
button here. You can click on it and you
2:13
add another screen to the canvas. So,
2:16
you can design a new page for what your
2:17
app will look like. and you know the
2:19
rest of it to redo, undo, delete and you
2:22
know plac in folders and stuff like
2:24
that. We don't want to get too detailed
2:25
in this video. I just want to walk you
2:26
through how simple it is to design your
2:29
application that you have in mind. That
2:32
dream application that's going to make
2:33
you like a million dollars and a
2:35
billionaire overnight. Yeah. So now like
2:38
I said, so this is the screen you can
2:40
come down. This is just the home screen.
2:42
Like this is a sample screen. As soon as
2:43
you open the application, you're going
2:44
to see this screen. So you when you
2:46
click on it, you're going to see the
2:48
details of the screen on the right hand
2:50
side here. You can of course toggle it
2:53
to um phone and desktop. You can rename
2:56
it to whatever you want like uh maybe
2:59
say something like dashboard or and you
3:02
can give it some kind of description
3:03
whatever you want. What you're doing
Turn a visual design into an AI prompt
3:05
right now is essentially you are
3:08
designing your application visually but
3:11
cleverly what you're doing at the same
3:12
time is writing a prompt for your AI
3:16
agent which is where this is leading to
3:19
actually it's not just the designing
3:21
part of it is the fact that the output
3:23
isn't just a design it is the exact
3:26
prompt you can hand over to your AI
3:28
agent to design this dream application
3:32
for you and potentially You can design
3:35
it for free. So, let's just hop right
3:37
back into what I was trying to say here
3:39
now. So, you could just give it a
3:41
description. You can probably write it
3:42
with AI. I'll get to that in a minute.
3:44
You can change the background colors,
3:46
you know, of the application to whatever
3:48
you like. You can, you know, format the
3:50
text and whatever. And then you can see
3:52
down here that this is the prompt it has
3:54
generated just for this UI screen alone.
3:58
It's given a very detailed prompt for
4:00
you which you can use as a as a prompt
4:02
for whatever agent or whatever tool you
4:05
use. So let's just head back to the left
4:07
hand side and take a take a look at what
4:09
we have over here now. So like I said,
Drag, drop and arrange components
4:12
this is where you can add new screens.
4:14
You have your components here from
4:16
different types of components like
4:17
buttons that can just you can easily
4:18
just click and drag a button to the
4:20
screen and it has this really nice
4:22
satisfying snap to it. As soon as you
4:24
put the thing on the layout, it just
4:26
snaps to the layout. So this um my UX
4:30
brothers out there, I know this will be
4:31
a blast for you. You can move anything
4:33
you like around. Just play with it. Like
4:36
this here, do whatever you like. Write
4:38
it as you put this here. And of course,
4:40
like I said, the prompt will change
4:43
accordingly based on the layout you have
4:46
for what your application should look
4:47
like. I don't want to get into each
4:49
individual component. There's loads of
4:51
them. You know, you can put a drop down,
4:53
you can put like a radio button, you can
4:56
just go crazy. what your app will look
4:58
like. Of course, you need to do your due
4:59
diligence. You need to know what you're
5:00
doing so that your prompt doesn't
5:02
produce a very terrible looking
5:04
application. But that being said, you
5:06
have the power to do whatever you like.
5:08
You see, I can move and place wherever I
5:10
want anywhere down here. So, that's that
5:13
covers the the tools and sorry, the
5:16
components you have. Now, you can also
Manage layers and groups
5:18
see the layout itself. You can see how
5:21
the layers are being stacked. You can
5:24
just you can actually move them around
5:26
in the panels. You can lock them so that
5:28
they don't move, you know. You can just
5:30
have it have a structure that you would
5:32
like, you know, arrange it in the
5:34
structure you'd like and see it here.
5:36
You can see this is even grouped
5:38
together. You have multiple um
5:41
components grouped into one particular
5:43
block. So of course that will help you
5:45
with hierarchies and it will also help
5:47
in the prompts. Now if you come back
Choose colours and themes
5:49
here, you also have colors. This way you
5:51
can set your color palette for what your
5:53
app will look like. If you click on it,
5:54
you see it even changes the entire color
5:56
palette of the entire UI itself to
5:59
match. But you know this is uh something
6:02
you can tinker with also. You can have
6:04
custom colors, change the color to
6:06
wherever you like. You get your hex
6:07
colors. You click on your color wheel
6:09
here and you can change the color
6:10
accordingly to what you want your apps
6:12
to look like. you I can just click
6:13
around and you can see that it's
6:15
changing the buttons, the impute fields,
6:18
your accent colors and stuff like that.
6:21
It's messing with it and it's pretty
6:24
simple. It's pretty straightforward and
6:25
you can toggle between light and dark
6:26
mode to see how the colors would feel
Adjust shape, type and motion
6:29
against both of those um modes. So come
6:33
back here. You will have your radius,
6:37
your um corner border radiuses or
6:40
whatever you call it. So you can control
6:42
how curved or how, you know, blocky it
6:45
is. For those that like um circled out
6:49
radi, you know, at the your buttons are
6:52
really rounded, your impute tools are
6:54
rounded, you could do that. If you're
6:56
someone that likes a little bit of uh
6:59
curve around your around the edges of
7:01
your impute fields, then you can also
7:03
have that going for you here. It's
7:04
pretty simple. Just click click. You can
7:06
see I've not done anything coding. I've
7:08
not done written a single line of code
7:10
to produce any of this output. This
7:13
beautiful this lovely milliondoll
7:15
application that I'm designing right
7:17
now. And over you can also change the
7:20
type face here. I know you're limited to
7:22
some of these type faces. But then
7:24
again, you have the prompt being
7:26
generated uh here. Let me just click on
7:28
that. You have the promptly generated
7:30
here. So you can always go in there and
7:31
change tell your AI to use whatever font
7:34
you like. But this is a list or a few
7:37
just tiny bits of fonts. Of course, they
7:39
can't go crazy and just uh tell them the
7:42
font you want to use. Basically, tell it
7:44
what we want to use. And down here you
7:46
will have your animation. How smooth
7:48
your animation would move. Do you want
7:50
standard animation? Do you want
7:51
expressive? It has that bouncer springy
7:53
animation to it. So your app will feel
7:56
very very responsive when you just drag
7:58
maybe an input field. It just bounces
8:00
around and just tickles you somewhere.
8:03
You can also do that here. And then of
Connect an optional AI helper
8:06
course I mentioned you have the AI thing
8:08
which I was going to talk about. So you
8:10
can connect your model directly to this
8:13
platform. Now this is from a security
8:16
standpoint. I don't know how safe it is
8:17
because you're just going to paste your
8:18
key into a
8:22
public facing website. I I have no idea
8:24
how they intend to handle the security
8:25
of that. But you know take that as you
8:27
will. I I wouldn't particularly want to
8:30
throw in my key here till, you know, I
8:32
do an extensive review, but it allows
8:35
you to bring your API or your AI agent
8:39
with you. Just plug it in there and
8:41
maybe have it look into it. Maybe like
8:43
there's an MCP that will just help you
8:46
pull this your prompt and then start
8:48
coding your application directly. But
8:51
I'm I have a lot of time on my hands.
8:53
I'm not that lazy. I could just easily
8:55
copy that prompt and then feed it to my
8:58
agent and that is it really. So you can
Build a restaurant app screen
9:02
just create another screen, come back
9:04
here and then just have a go. Just do
9:07
whatever you like, you know, to pop in
9:09
another title here. You see how snappy
9:12
it is. Pop in like a search here. Maybe
9:14
if you have a restaurant or something,
9:16
you can you can see how this would look
9:19
for what you want. They have a card here
9:21
somewhere. You can just Yeah. So,
9:22
there's a card. Just pop in a card here.
9:25
Maybe you have like a menu item here.
9:28
And
9:29
uh let's say uh Jolof
9:32
rice and say something like uh Nigerian
9:37
uh Jolof is better than
9:42
the rest, you know, stuff like that. I
9:45
don't know. And then you can have a
9:47
button here that just says order
Share the design with your team
9:51
now. So you can see how simple it is to
9:54
just design your application. This is
9:56
just a few like I told you seconds you
9:57
can design it and you can send this over
10:00
to your UI person, your UX person. You
10:03
can send it to whoever. You can take
10:04
screenshots of it and have it in your
10:07
product review or your or your meetings
10:08
where you can showcase what you you
10:10
think the product should look like or
10:12
should have, what features it should
10:13
have. And that will be really visual to
10:16
whoever is looking at it. They will
10:18
understand where you're coming from. Not
10:20
like you're trying to describe something
10:21
that you cannot really describe. They
10:24
can see it for themselves and decide if
10:25
that's the approach they want to go in
10:27
as regards to the design of the
10:28
application. And like I said, it is
10:30
responsive. You can see how it looks on
10:32
a tablet or on a laptop screen. And that
10:35
is proper. Anyway, so this is the this
10:39
is the prompt. And you can see uh that
Export the prompt for a coding agent
10:41
it's also for it's Android and web. But
10:44
don't let that discourage you as an iOS
10:46
person because this is just a template.
10:48
You can just tell your coding agent to
10:50
instead of use Android because like I
10:52
said output is a prompt instead of doing
10:54
it in Android just do it in iOS. Use
10:56
Swift UI, use Swift, use UI kit,
10:59
whatever you like and build it out and
11:02
or for web you can tell it whatever use
11:04
like use um React for it, use spelt for
11:07
it. Use spelt actually just use spelt.
11:09
Anyway, you can tell your your coding
11:12
agent to do whatever you like. You have
11:13
the prompt. It is just text. It is not
11:15
code. You can see the colors are defined
11:17
here. If I scroll down, you can see the
11:19
shape. You can see the layout. You can
11:22
see the dashboard I defined for what the
11:24
page, this particular page will be. And
11:27
whatever I do, it just regenerates this
11:30
prompt for me. And you can be very
11:32
detailed. You can take this prompt out,
11:34
read it, improve it, because now you
11:36
have a very standard template of what
11:38
your app would do and what it will
11:40
function like. And then you just feed
11:43
that to whatever your agent is. You can
11:45
give it an app name, you know, my great
11:49
app. Sorry, I don't know what's wrong
11:51
with my pants today. And that's it. Down
11:55
here, you can just click on copy prompt
11:56
and you're good to go. And I think this
Final thoughts on M3E Canvas
11:59
is a brilliant product for whoever this
12:03
is for. I know you know who you are. If
12:06
you like if you like this product, I
12:07
think you should just hop on it, get on
12:08
it, and start to use it. Anyway, this
Outro and next video
12:11
brings me to the end of this video. It
12:13
was a short video. Hopefully, it's a
12:15
short video. And if you like to see more
12:17
videos like this, please like, share,
12:19
and subscribe to the channel. And if you
12:21
want to see the video on why I imposter
12:24
syndrome has just been crazy in the tech
12:27
world, please watch the video here.
12:29
Otherwise, I'll see you guys in the next