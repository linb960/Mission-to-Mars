# Mission to Mars

## Overview 
This project creates a webpage of information and images of Mars.<br>

Using BeautifulSoup and Splinter full-resolution images of Mars’s hemispheres and their titles are scraped from a website.  In addition some news and facts about Mars is also scraped from other websites and all of this is displayed on a webpage using Flask.

## Notes about Project
The work of scraping the various websites is done in scraping.py and the index.html sets up the containers used to layout the page.  The python code in app.py sets up the routes.  The route for the index will run a function to get data from the mongo database table mars and display it.  A button on the page will call the /scrape route function which calls the scraping.py code to collect all of the elements for the mongo database.<br><br>

The full scale images collected specifically for this project are displayed at the bottom of the page and the rest of the page comes from the work done in Module 10.<br>

Deliverable 3 asked for two additional Bootstrap features on the page.
1. The button is styled
```
 <!-- Add a button to activate scraping script -->
        <p><a style="border-width: 1; border: solid; text-align: center; color:rgb(0, 2, 128);" class="btn btn-primary btn-lg" href="/scrape" role="button">Scrape New Data</a></p>
```

2. Thumbnail images are displayed.  To do this first code was added to scraping.py to get the thumb_link
```
 try: 
            # Get the thumbnail image link
            thumb_link = hemisphere_soup.select("div.item img")[i].get('src')
        except AttributeError:
            return None
 ```
 The thumb_link was added to the dictionary for the database
 
 ```
  # Add extracts to the results dict
        hemispheres = {
            'img_url': img_url,
            'title': img_title,
            'thumb': thumb_link}
 ```
 Then in the index.html a section was added to make the thumbnails and titles into Bootstrap figures and the figures were put in a table.
 ```
 <div class="table-responsive" id="mars-hemispheres-thumbs">
        <h2>Mars Hemispheres Thumbnails</h2> 
          <table class="table">
            <tr>
                {% for hemisphere in mars.hemispheres %}
                <td>
                  <figure class="figure">
                    <img src="{{hemisphere.thumb}}" class="figure-img img-fluid rounded" alt="Mars Hemispheres">
                     <figcaption class="figure-caption">{{hemisphere.title}}</figcaption>
                   </figure>
                 </td>
                {%endfor%}
            </tr>
          </table>
      </div>
```
## Issues:
- Sometimes when the data is scraped the webpage shows none in the news and or the table of facts.  This happens because if there is an error in exception caught in the try block it returns none.
- The page is responsive on small displays except for the table of thumbnails.  I used the "table-responsive" class for the div but the table does not collapse into a vertical column.   More work can go into this but for now I'm happy to have acheived getting the thumbnails on the page.
