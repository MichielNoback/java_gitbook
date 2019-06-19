# Exercises intro

These exercises accompany the course Web-based Information Systems.

----

# Implementing MVC 

**Goal:** Create a web application that gives the heart rate zones based on the maximum heart rate.

#### Background

The maximum heart rate can be determined by running (preferably on a treadmill but outside will do) as fast as you can for 3 minutes straight, followed by a three-minute gentle run and again 3 minutes full force. Optionally, add another cycle.
The maximum reading is the maximum heart rate.

The heart rate zones can be calculated by this rule set, giving zone boundaries and descriptions as % of maximum heart rate: 
- zone 1: 50-60% Low heart rate with hardly any training effect
- zone 2: 60-70% Heart rate with positive effect on breathing and stress reduction
- zone 3: 70-80% Heart rate with optimal training effect with respect to fat burning, general condition and brain functioning
- zone 4: 80-90% Zone for improving (long-distance) running speed
- zone 5: 90-100% Above anaerobic threshold; only during interval training to increase speed (also for shorter distances).

#### The exercise
Start by creating a new Gradle-managed web project.  

Next, create the servlet dealing with the url `/heart-rate-zones`. Its `doGet()` method should forward to a Thymeleaf html page with a form asking the user for her maximum heart rate (and giving usage instructions). It should support both Dutch and English. The form should direct back to the servlet serving url `/heart-rate-zones`, but this time to the `doPost()` method. Here, the servlet should invoke a model class responsible for calculating the correct heart rate zones.

Next, these zones should be displayed in another html Thymeleaf view, again supporting the languages Dutch and English.

All texts should be located in the resource bundle(s), not in the html and neither in the Java classes.

Create appropriate packages for your Java classes.

Note: yes this could have been done more efficiently using Javascript, but we are working with Java for now.

**_[Challenge]_** Support multiple languages in this app.

----

# Thymeleaf

**Goal:** Create a web application that publishes information on some species from your favorite animal or plant group.

#### Background
This assignment will encourage you to use as many Thymeleaf expressions as possible, as well as resource bundles.  

As preparation, you should choose an animal or plant group that you think is interesting. From this group of organisms, 
choose 5 representatives and gather some relevant information, as well as at least one picture.
The information should include at least:
- Scientific name
- English name
- Dutch name

Here is an example of one of my favorite birds, the steppe eagle (copied from Wikipedia):


**Steppe eagle (_Aquila nipalensis_)**  
**Dutch name**: Steppearend

![Steppe eagle](figures/steppe_eagle.jpg)
(By T. R. Shankar Raman - <span class="int-own-work" lang="en">Own work</span>, <a href="https://creativecommons.org/licenses/by-sa/3.0" title="Creative Commons Attribution-Share Alike 3.0">CC BY-SA 3.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=60338791">Link</a>)

**Description**  
The steppe eagle (_Aquila nipalensis_) is a bird of prey. Like all eagles, it belongs to the family _Accipitridae_. It was once considered to be closely related to the non-migratory tawny eagle (Aquila rapax) and the two forms have previously been treated as conspecific. They were split based on pronounced differences in morphology and anatomy.
Wing span: 1.65–2.15 meters  
**Occurrence**  
It is a migratory bird breeding in eastern Europe, the Middle East and Asia and wintering in Africa and South Asia.  
**Conservation status**  
Threatened (declining populations)


#### The exercise

1. Start by creating a new Gradle-managed web project.  
2. Create a simple "csv" file in a data folder that holds, for each species, all non-textual data. Here is my Steppe Eagle example:

    ```
    scientific_name;english_name;dutch_name;wing_span_avg;picture_loc
    Aquila nipalensis;Steppe Eagle;steppearend;1.9;figures/steppe_eagle.jpg
    ```

3. Collect all other (textual) data in a resource bundle, supporting both English and Dutch.

4. Design and implement Java classes that model your information.

5. Create a class that can parse your csv file into a list of instances representing your species.

6. Create a servlet serving url `/home` that redirects to a Thymeleaf page called `listing.html`. 
This page should list all available species in a table, providing for each species a link to a species-detail page.
This page is internationalized of course.

7. Create a servlet serving url `/species.detail` that directs to a Thymeleaf page showing the detailed information of a selected species (linked via the table created before).
This page is also internationalized.

8. **_[Challenge]_** Use Bootstrap to make the two pages look nice as well, or apply your own styling.

You are of course free to extend the information with whatever you think is nice.  

----

# Sessions and scopes

**Goal:** Modify a web application to support browsing history.

#### Background
The web app that was developed in the previous exercise is very static and impersonal. We'll add some functionality to fix that.

#### The exercise

