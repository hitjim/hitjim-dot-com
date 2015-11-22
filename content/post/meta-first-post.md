{
   "categories": ["hugo", "github"],
   "date": "2015-11-21T23:33:51-06:00",
   "description": "How I got my website running with Hugo and Github Pages",
   "keywords": ["blog","guide", "notes", "github", "hugo"],
   "title": "Meta First Post - Setting Up a Personal Site with Hugo and Github Pages",
   "slug": "yawwwn-meta-first-post-setting-up-hugo-on-github-pages"
}
I'd done it a half dozen times before.  Scorch the earth on my webserver, start a new database and spin a new Wordpress blog with the purest of intentions.  But this time would be different!  It always is.  

### THIS TIME *IS* DIFFERENT!  I'M GOING TO USE GHOST!!
I've used Ghost a few times on teams and it's really nice.  Not throwing stones here.  But I had that nagging feeling as I dug into guides for setting up Ghost on Digital Ocean.  I fretted over which Linux distro to use, or whether to allow comments.  Tangents and rabbit holes abound.

### I don't *need* that.
I need simplicity.  I need speed.  I need ease or near-absence of maintenance.  I need to type words and have them display on a screen.  I need a static site.

I'd heard great things about Jekyll, and if I hadn't recently been learning Go, maybe I would finally be playing with Ruby.  But today is not that day.  So [Hugo](https://gohugo.io) it was.

I'd noticed a few peers were hosting their blogs and portfolio sites on [Github Pages] (https://pages.github.com/).  Awesome, source control plus free webhosting.  Now I don't have to shell out money each year to *not* update my blog.  Wait... I mean, *this time will be different!!*

So, I quite easily found [this guide](https://gohugo.io/tutorials/github-pages-blog/) for using Hugo to publish static websites on Github Pages.

### Words are for reading.
I hope I will continue adding content to this site in the same way you see here.  I may ramble at bit at times.  But I will *not* just share my victories and paint them as flawless.

I wasted a good evening trying to follow the steps at the top of the guide linked above, which was intended to establish a `gh-pages` branch for a project repo.  That was NOT what I wanted.  It's no big surpise that I got build errors when I tried to have a `gh-pages` branch in a repo named such that it gets automatically built as a github page.  I scrolled further into the guide to find the steps accurately and obviously titled "Host Personal/Organizational Pages."  After swearing at myself and some pro-level face-palming, I continued on my quest.

### Here's the gist.
1. Download and install [Go](https://golang.org/doc/install) and [Hugo](https://gohugo.io/overview/installing/).
1. Create Github repo for building site and authoring content - https://github.com/hitjim/hitjim-hugo
1. Create Github repo for hosting site.  You can witness the litany of errors in the early commit history! - https://github.com/hitjim/hitjim.github.io
1. Locally clone and enter `hitjim-hugo` repo, [install a theme I liked](https://github.com/zyro/hyde-x#installation).
1. Then ape the [config file from the author of that theme's blog](https://github.com/zyro/andreimihu.com/blob/master/config.toml) for my own config file at the root dir of the project, while making the appropriate personalizations.
1. Run `hugo server --watch -t hyde-x` to start a local dev server that watches and updates when it detects changes.
1. Run `hugo new about.md` to make and then edit my about page.
1. Run `hugo new post\meta-first-post.md` to to make and then edit this very post.
1. Delete the `public` folder as instructed and run `hugo` on its own for the final build.
1. Copy contents of `public` to the `hitjim.github.io` repo, create a CNAME file containing my only my domain name `hitjim.com`, and push changes to Github.
1. Configure my domain registrar to play nicely with Github Pages.  Much of [this page](https://help.github.com/articles/my-custom-domain-isn-t-working/) helped me get it right.  In my case, I just needed "A Record" type entries, one with host `@` and another with `www`, and each one pointing a static IP for Github Pages - `192.30.252.153` and `192.30.252.154`. 
1. Be possibly a little too pleased with myself. 
