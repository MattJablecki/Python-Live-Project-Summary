Django Two Week Live Project

## PROJECT OVERVIEW

To cap off the Python Course offered at The Tech Academy, I took part in a live Django project, teaming up with other students to build a web scraping application.  The ultimate goal of the application was to gather data from the web that the Project Director deemed relevant to the user.  This included weather conditions, a current box office top 5 list, traffic reports, a concert calendar, and reports from various space programs. These tasks were sectioned off into "User Stories", which were added as necessary and up for grabs on a first come, first serve basis. My group, which initially consisted of myself, another student, and the Project Director, eventually ended up as a group of six. Together we collaboratively compiled code with the intended purpose of creating a unique and robust user experience. Below you'll find my contributions to the project, including photos and blocks of code. 

## Weather App

Harnessing the Open Weather Map API, I created an application in which the user could enter any valid 5 digit zip code. As a result, the name of the city, the current temperature, the description of the conditions, and a corresponding icon would appear on the screen. This zip code would then be stored in their database, giving up to date info for that city on each login to the app. 
```
def weather(request):
    url = 'https://api.openweathermap.org/data/2.5/weather?zip={}&units=imperial&appid=717773b8d51cee768b8ceb819ad9aeb3'
  
    if request.method == "POST":
        form = ZipForm(request.POST)
        form.save()

    form = ZipForm()

    zips = Zip.objects.all()

    weather_data = []

    for zip in zips:

        r = requests.get(url.format(zip)).json()
        
        zip_weather = {
            'city' : r['name'],
            'temperature' : r['main']['temp'],
            'description' : r['weather'][0]['description'],
            'icon' : r['weather'][0]['icon']
        }

        weather_data.append(zip_weather)

    print(weather_data)

    context = {'weather_data' : weather_data, 'form' : form }

    return render(request, 'weather/weather.html', context)
```
## Geolocation

Utilizing my understanding of APIs, I used the same logic applied to the Weather App to call the IPGeolocation API to gather the user's City, State, and Zip Code. This was my last task on the project, so although I didn't get to fully utilize its potential, I left the code for the next group to use as they saw fit. 
```
def location(request):
    url='https://api.ipgeolocation.io/ipgeo?apiKey=b19ee562aa72436e94a39580c8265ad3'
    
    r = requests.get(url).json()

    location_data  = {
        'city' : r['city'],
        'state' : r['state_prov'],
        'zip' : r['zipcode']
    }

    print(location_data)
    context = {"location_data" : location_data}

    return render(request, 'location/location.html', context)
```
## Front End User Stories

I also handled the lion's share of the front end work, as myself and my fellow team member were starting on a fresh project. Listed below are the parameters of the stories and some resulting images for context. 

```
Landing/Home Page Separation

Description
User:
"When I first visit the site, if I'm not logged in, I only want to see a 
minimalist page with login/signup options. When or if I am signed in, 
I'd like to see my dashboard with my apps and menu options."

Acceptance Criteria
The landing page is only shown if the user is not logged in. 
The landing page is Very Minimal.
```

```
Base Template for Front End

We need a base template to start working off of for our front end.  Create a base 
template with a link to bootstrap 3.  Also create a css stylesheet for the project 
and link it into the base template. 

- In settings.py add paths for holding and serving static files.
- Add a folder to the path specified for STATICFILES_DIR
- Run 'python manage.py collectstatic' command in the terminal
- Add load static template tags to base.html
- Add Bootstrap 4 CDN to head 
- Create a base.html template within the templates folder
```

```
Dashboard - Styling
Description 
User:
"I'd like the pages of this site to look like a dark color theme of 
an IDE so it feels like a private hacker tool."
```

```
DashboardApp - Front end
Description
User:
"I want to see all of my Apps displayed as modules on the 
same page when I log in to my account"
```

```
Logging In/Out as User
Description: As a user, I can log into and out of the application. There will 
be a dedicated login page, and a logout link will be visible to the user on all 
pages while they are logged in.

- Add a registration folder inside of the templates folder, and create a login.html page.
- Add to urlpatterns in DataScrape urls.py file
- Displaying Navigation Links (Create an if/else statement that will render the correct navigation links.
If a user is authenticated, render this line of code, else, render that link...)
- Decide what happens when a user successfully logs in or out.
```
Add Images

## Summary
Over the course of the two week sprint, I was presented with a number of challenges that I had never faced previous to this experience. While at times difficult, ultimately this sharpened my skills and helped me develop new ones. Additionally, I got to work in a real world simulation, collaborating with other developers, communicating needs and troubleshooting obstacles as a group. This is a major facet of engineering and I'm grateful for the opportunity. While I was sad to leave the project before completion, I was proud to pass along what I accomplished to the next group and hope it serves them well. 
