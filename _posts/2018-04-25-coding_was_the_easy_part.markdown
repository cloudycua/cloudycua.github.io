---
layout: post
title:      "Coding was the easy part"
date:       2018-04-25 14:30:01 +0000
permalink:  coding_was_the_easy_part
---


I pretty much finished the coding requirements for my CLI Gem Project while I was waiting for approval of my idea. Procrastinating about writing my blog and preparing a video demo is taking infinitely longer (which I have yet to do.) While the non-coding portions of the project have been an albatross, not helped any by my doing the course part-time and thus easily turning my attention to other things I need to do and things that are less urgent, they are beneficial in that they forcing me to present myself and try new things. Who knew that Xbox would come in handy for capturing a coding session and my future video demo?

Best-Picture-Winners is my CLI Gem Project that creates a database of Academy Award Best Picture winners using details from Rotten Tomatoes. The user can choose to see the list of winner alphabetically, by year, by Tomatometer (Rotten Tomatoes critic ratings), or by Audience Score. They can they select a winner by year or title to see additional details: genre, film rating, runtime, synopsis and critics consensus.

The list of award winners comes from a Rotten Tomatoes article:

https://editorial.rottentomatoes.com/guide/oscars-best-and-worst-best-pictures/

#### Scraping

While this project was more about the CLI, I enjoyed scraping the data and working through how to get the details I wanted. I scraped the title, year and url from the RT article (and the 8 other pages that list all the winners), then used the urls to find the individual movie pages to scrape the rest of the data. Overall, the scraping was pretty easy, but there were a few issues I had to work through. The great thing is that there is so much information out there and coder helping eachother, that it is easy to quickly google for a solution. The first was learning to using the CSS selector to get the element attribute value for the url link:

`<**h2**><**a href**=**'https://www.rottentomatoes.com/m/broadway_melody/'**>The Broadway Melody</a>`

`movie_url = movie.css**("h2 a").attr("href").value**`

Then there was figuring out how to get rid of new lines and extra whitespace in many of the details. `delete!(obj)` allows you to remove a specific item, `strip` removes the leading and trailing whitespace.

`attribute.css(".meta-value").text**.delete!("\n").strip**`

I originally didn't include the film ratings, genres, or runtimes or in the movie details because I couldn't figure out how to scrape the specific data. 

```
<ul class="content-meta info">
    <**li **class="meta-row clearfix">
            <div class="**meta-label** subtle">**Genre: **</div>
            <div class="**meta-value**">
                <a href="/browse/opening/?genres=12">
                        **Musical & Performing Arts**</a>, <a href="/browse/opening/?genres=18">
                        **Romance**</a></div>
        </li>
    <**li** class="meta-row clearfix">
            <div class="**meta-label** subtle">**Runtime:** </div>
            <div class="**meta-value**">
                <time datetime="P104M">
                    **104 minutes**
                </time>
            </div>
        </li>				
```

Then I realized the lists were basically arrays.

```
movie.css("**li**").each do |attribute|
      if attribute.css("**.meta-label**").text.strip == "**Genre:**"
         if attribute.css("**.meta-value**").text.delete!("\n") != nil
             BestPictureWinners::Movie.all[index].genre = attribute.css**(".meta-value").text**.delete!("\n").delete(" ").gsub(",", ", ")
         end
      end
      if attribute.css("**.meta-label**").text.strip == "**Runtime:**"
          if attribute.css("**.meta-value**").text.delete!("\n") != nil
              BestPictureWinners::Movie.all[index].runtime = attribute.css**(".meta-value").text**.delete!("\n").strip
          end
      end
end
```

The last scraping issue that came up was getting back double or triples of the data I was scraping for. Since the page is fluid and the formatting changes based on the browser size, the html code was sometimes duplicated multiple times with the only difference being the class description for the column size. So, that became a crucial CSS selector.

`movie_page.css("div#scorePanel **.col-full-xs** p.critic_consensus").text.gsub("Critic Consensus:","").lstrip`


### CLI

The code for my project can be found here:

[https://github.com/cloudycua/best-picture-winners-cli-gem](https://github.com/cloudycua/best-picture-winners-cli-gem)

I liked the way `bundle gem` to set up all my files and directories. After making a few modifications to files like `README.md`, pretty much all my work was done on the files inside my `lib/best_picture_winners` directory. I initially started stubbing out my CLI file with the code I wanted to write assuming fake data, but since I was still waiting for approval, I didn't want to write a bunch of code for a project that didn't get approval or had different requirements. So, I decided to play with scraping data since it would be good practice either way. In some ways, I found it easier to start with the scraping since I then knew which data I had to work with and what format it came in.

The first scraper file I set up, `scraper_url.rb`, scrapes the movie title, year and url from Rotten Tomatoes' guide to the best and worst Oscar pictures, and creates a new movie object with these attributes using the Movie class from the `movie.rb` file. The second scraper file, `scraper_info.rb`, fleshes out the other movie attributes like genre, tomatometer, audience score, synopsis, etc. 

The CLI file pulls it all together and calls on the two scraper files to create the movie database. It then asks the user a series of questions of how they would like the movie list sorted: alphabetically, by year, by Rotten Tomatoes' Critics Tomatometer or Audience Score. It then calls on the movie file to sort and print the movie list. After printing the movie list, the user is asked if there is a particular movie or year that they would like to see more details for.

While numerous lessons have touched on all the different parts of the gem, this project puts all the pieces together and really forces you to see how everything works together and how all the different files and folder are really just an organization system which you can move back and forth from pulling whatever class or method you need at a particular instance.
				

