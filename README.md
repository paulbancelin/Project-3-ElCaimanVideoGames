# Project-3-ElCaimanVideoGames


### Goal

The goal of this project is find the best place for a new company in the gaming industry and consider some criterias:

- Designers like to go to design talks and share knowledge. There must be some nearby companies that also do design.
- Developers like to be near successful tech startups that have raised at least 1 Million dollars.
- Executives like Starbucks A LOT. Ensure there's a starbucks not too far.
- Account managers need to travel a lot.
- Everyone in the company is between 25 and 40, give them some place to go party.
- The CEO is vegan.
- If you want to make the maintenance guy happy, a basketball stadium must be around 10 Km.




### Database description

The database refers to a 18.8k documents where store startups from around the world and each startup has some data abaout it that it will be used to stablish the new company in a proper area.


### General cleaning

First of all consider the criterias:

-Developers like to be near successful tech startups that have raised at least 1 Million dollars. And I also added another criteria, which is that the acquisition price of the company had to be at least 1 million

-Designers like to go to design talks and share knowledge. There must be some nearby companies that also do design. For this one I consider that all the video games startups have also designers. 


For the criterias above I apply the following coding:

#### 1.

len(list(c.find({"category_code":"games_video"})))

That gave me 1083 startups that are in the video game industry.

len(list(c.find({
    "category_code": "games_video",
    "acquisition.price_amount": { "$gte": 1000000 }
})))


#### 2.

That gave me 42 startups that are in the video game industry and the acquisition price is 1 million.


#### 3.

query = {"category_code": "games_video",
    "acquisition.price_amount": { "$gte": 1000000 }}
projection = {"_id":0, "name":1, "acquisition.price_amount":1, "offices.city":1, "offices.latitude":1, "offices.longitude":1, "offices.country_code":1, "offices.zip_code":1, "total_money_raised":1}
results = list(c.find(query, projection))
results


Where the idea is to see in a Data Frame all the 42 startups with their respective total money raised that was reduced to 39 as 3 of them havent raise at least 1 millon. 

#### 4.


And to locate them I use a heat map:


mapa = folium.Map(location=[df['latitude'].iloc[0], df['longitude'].iloc[0]], zoom_start=2)


heat_data = [[row['latitude'], row['longitude']] for index, row in money_raised_df.iterrows() if row['latitude'] is not None and row['longitude'] is not None]
HeatMap(heat_data).add_to(mapa)
mapa



As the two createrias above where the most important for establish the company I choose San Francisco, California, USA.

#### 5.

Then I locate where are the places to meet the criteria that the staff needs using the coordinates of San Francisco in foursquare and the following function:


token = getpass.getpass(prompt='Enter your token: ')


def foursquare_data(data):
    rows = []
    for result in data['results']:
        row = {
            'name': result['name'],
            'distance': result['distance'],
            'latitude': result['geocodes']['main']['latitude'],
            'longitude': result['geocodes']['main']['longitude'],
            'category_id': result['categories'][0]['id'],
            'category_name': result['categories'][0]['name']
        }
        rows.append(row)
    df = pd.DataFrame(rows).sort_values(by='distance', ascending=True)
    return df
    
    
    url =  'Api Link' 

headers = {
    "accept": "application/json",
    "Authorization": token }
response = requests.get(url, headers=headers)
response

raw = response.json()
place = foursquare_data(raw)
place


#### 6.

Finally generate visualizations of the locations that the places of the staff needs on a map using:

for i, row in place.iterrows():
    folium.Marker(location=[row['latitude'], row['longitude']], 
                  icon=folium.Icon(color='color', icon='figure', prefix='fa')).add_to(map)
map    


### Conclusions

The right coordinates for the location of the gaming company are:

latitude = 37.7749
longitude = -122.4194


As they meet in general all the criterias that the staff need.


 







