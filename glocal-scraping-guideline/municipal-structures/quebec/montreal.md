# Montreal

Great. I’ll prepare a concise yet data-focused breakdown of Montreal’s municipal structure as of 2025, with an emphasis on the composition of the 65-member City Council and the nuances that may cause confusion in scraping or search results (e.g., duplicated roles or borough-specific councils). I’ll include source suggestions for up-to-date data, and embed scraping tips using the `{tip}` format.

I’ll let you know when it's ready for review.

# Montreal Municipal Government Structure (2025)

Montreal’s municipal government is organized into a central **City Council** and multiple **borough councils**, reflecting a two-tier system. The city is divided into 19 boroughs (*arrondissements*), each with its own local council, in addition to representation on the city-wide council. Understanding this structure is key to correctly interpreting the number of councillors and avoiding confusion when scraping data or comparing sources.

## City Council: Composition and Roles

The **City Council** is Montréal’s primary decision-making body and as of 2025 it consists of **65 members** ([Roles of elected officials - Élections Montréal](https://elections.montreal.ca/en/roles-of-elected-members/#:~:text=The%20City%20of%20Montreal%20has,103%20elected%20officials)). This total includes the following:

- **1 Mayor of Montreal** – The city’s mayor (who *also* serves as the borough mayor for Ville-Marie) ([Roles of elected officials - Élections Montréal](https://elections.montreal.ca/en/roles-of-elected-members/#:~:text=,46%20city%20councillors)).
- **18 Borough Mayors** – One for each of the other 18 boroughs (since Ville-Marie’s borough mayor is the city mayor). These borough mayors are simultaneously city councillors for their boroughs.
- **46 City Councillors** – Regular city council members representing electoral districts across the boroughs ([Roles of elected officials - Élections Montréal](https://elections.montreal.ca/en/roles-of-elected-members/#:~:text=,46%20city%20councillors)).

In other words, Montreal’s city council is made up of the **mayor and 64 councillors** (including those who are borough mayors) ([Montreal City Council - Wikipedia](https://en.wikipedia.org/wiki/Montreal_City_Council#:~:text=The%20current%20city%20council%20consists,entities%20at%20the%20municipal%20level)). The mayor of Montreal is an ex officio borough mayor for Ville-Marie, which means no separate election is held for a Ville-Marie borough mayor – this unique situation is why we count 18 borough mayors rather than 19. Each of the 64 councillors (borough mayors or district councillors) has a vote on city council alongside the mayor.

{tip}When scraping council data, remember **the Mayor will often be listed with two roles** (City Mayor and Borough Mayor of Ville-Marie). Be careful **not to double-count** this person. Treat “Mayor of Montreal (Ville-Marie Borough)” as one individual in the city council lineup.{tip}

## Borough Councils and Total Elected Officials

Each borough in Montreal has its own **borough council** that handles local matters (parks, permits, local roads, etc.) separate from the city council. A borough council is composed of the borough’s mayor and a set of councillors from that borough. By law, **borough councils have at least 5 members** ([Roles of elected officials - Élections Montréal](https://elections.montreal.ca/en/roles-of-elected-members/#:~:text=All%20elected%20officials%20of%20the,have%20at%20least%205%20members)). These members typically include:

- The **Borough Mayor** (who is also on the city council, except in Ville-Marie where it’s the city mayor).  
- The **City Councillor(s)** elected for that borough’s district(s) (if not already the borough mayor).  
- Additional **Borough Councillors** elected to represent local districts **only at the borough level** (they do **not** sit on city council).

This structure means not all elected officials in Montreal sit on the 65-member city council. Some are **borough-only councillors**. **In total, Montreal has 103 elected officials city-wide** when you include both city council and borough councils ([Roles of elected officials - Élections Montréal](https://elections.montreal.ca/en/roles-of-elected-members/#:~:text=The%20City%20of%20Montreal%20has,103%20elected%20officials)). The 65 city council members are a subset of this, and the remaining are the borough-level councillors who only vote within their borough. For example, a large borough might have a borough mayor and several city councillors on city council, plus a few extra borough councillors to meet local needs. In Ville-Marie, since the city mayor fills the borough mayor role, its borough council includes the mayor and a few appointed city councillors from other areas to reach the required size ([Montreal City Council - Wikipedia](https://en.wikipedia.org/wiki/Montreal_City_Council#:~:text=Each%20borough%20is%20divided%20into,additional%20borough%20councillors%2C%20as%20follows)) ([Montreal City Council - Wikipedia](https://en.wikipedia.org/wiki/Montreal_City_Council#:~:text=elects%202%20borough%20councillors%20Ville,boroughs%20named%20by%20the%20Mayor)).

```{admonition} **Scraping Tip**
:class: tip
If you’re extracting data and see **over 65 names**, you’re likely getting **borough council members** too. To isolate just the city council, **filter out titles like “borough councillor”** and keep only the mayor, borough mayors, and city councillors. This will ensure you have the 65 officials who sit on the city council ([Too many councillors in Montreal? - Spacing Montreal | Spacing Montreal](https://spacing.ca/montreal/2011/01/17/too-many-councillors-in-montreal/#:~:text=So%20how%20does%20our%20City,councillors%2C%20despite%20being%20having%20over)).
```

## Why the Council Member Count Can Vary by Source

It’s important to note why different websites or data sources might show varying counts of officials:

- **City Council vs. All Elected Officials:** Some sources explicitly state Montreal City Council has 65 members (which includes the mayor) ([Montreal City Council - Wikipedia](https://en.wikipedia.org/wiki/Montreal_City_Council#:~:text=The%20current%20city%20council%20consists,entities%20at%20the%20municipal%20level)). Other sources might mention **64 councillors plus the mayor**, which is just another way to describe the same 65-member council. In contrast, a list of *all* elected officials in Montreal (city + borough level) will list 103 names, which can be mistaken for “103 councillors” if one isn’t aware of the borough system ([Roles of elected officials - Élections Montréal](https://elections.montreal.ca/en/roles-of-elected-members/#:~:text=The%20City%20of%20Montreal%20has,103%20elected%20officials)). Always check if a source is referring only to city councillors or to **both city and borough officials** combined.

- **Dual Roles and Listings:** Because some individuals hold dual roles (e.g. the Mayor of Montreal is also a borough mayor, and each borough mayor is also a city councillor), they might appear in multiple categories on a website. The official city website, for instance, might list the Mayor on a dedicated page and also list that person under Ville-Marie’s borough representatives. This could lead a simple scrape to count the same person twice if not careful.

- **Outdated or Different Terminology:** Ensure the data is up to date. Montreal’s political structure has been stable in recent years with 65 city council members, but if you come across older information (e.g. prior to borough mergers or changes in 2006), the numbers or borough names might differ. Similarly, French-language sources will use terms like *conseiller de la ville* (city councillor) and *conseiller d’arrondissement* (borough councillor). An English site might just say "councillor" for city councillors. This terminology can affect search or scrape results if not accounted for.

```{admonition} **Scraping Tip**
:class: tip

When scraping names and roles, use the role labels to your advantage. For example, Montreal’s open data or websites might tag each official as **Mayor**, **City Councillor**, or **Borough Councillor**. Use these tags to **filter out borough-only officials**. Also note that borough names are in French (e.g. *Côte-des-Neiges–Notre-Dame-de-Grâce*); be consistent in using the same names or IDs when filtering by borough.
```

## Reliable Data Sources for Scraping

To get accurate and current data on Montreal’s council, consider these official sources:

- **City of Montreal Official Website (montreal.ca):** The site provides pages for city council and elected officials. For instance, the “City Council” page confirms the composition and roles ([City council | Ville de Montréal](https://montreal.ca/en/city-government/city-council#:~:text=City%20council%20is%20composed%20of,65%20elected%20officials)). The “Elected Officials” section lists all council members and borough councillors (with options to filter by borough or role). This is a trustworthy source, though the content may be dynamically loaded. Scraping it might require handling JavaScript or using their JSON endpoints, if available.

- **Élections Québec / Élections Montréal:** After municipal elections, the official election authorities publish the results and the list of elected officials. Élections Montréal (the city’s election office) provides details on the positions and can be consulted for authoritative counts and any mid-term changes. The election site (or Élections Québec, which oversees provincial municipal election data) will have the final list of all winners for each borough and city position. This is useful for verifying the number of councillors and their roles as of the last election.

- **Open Data Portals:** Montréal’s open data portal (often via [Donnees Quebec](https://donnees.montreal.ca) or the Canadian Open Government portal) offers a machine-readable **dataset of elected officials**. For example, a CSV list of Montreal’s elected officials (updated for the 2021–2025 term) is available ([List of elected officials of the City of Montreal - Open Government Portal](https://open.canada.ca/data/en/dataset/381d74ca-dadd-459f-95c9-db255b5f4480#:~:text=Montreal.%202025,csv%20List%20of%20elected%20officials)). This dataset includes each official’s name, role, and borough, and is ideal for developers. By using the open data, you can directly obtain the structured list of the 65 city council members and the borough councillors without scraping the website’s HTML. 

Using these sources will help ensure you get the **correct count and up-to-date names**. Always cross-reference if possible – for instance, if scraping the city website, you might compare the results against the open data file to verify that you’ve captured all 65 city council members and not accidentally included or omitted others.

```{admonition} **Scraping Tip**
:class: tip
 The open data CSV (or JSON, if provided) is typically in French. Field names like “**poste**” (position) will indicate if the person is *Maire*, *Maire d’arrondissement*, *Conseiller de la ville*, or *Conseiller d’arrondissement*. Leverage these fields to programmatically distinguish roles. Also, ensure your scraper handles **accented characters** in names and boroughs (e.g., *É* in Élections, *Côte-des-Neiges*). This will prevent encoding issues when you compile the data.
 ```

By understanding Montreal’s council structure and using the above sources, you can reliably scrape or reference the **65 city council members** (1 Mayor, 18 borough mayors, and 46 councillors) ([Roles of elected officials - Élections Montréal](https://elections.montreal.ca/en/roles-of-elected-members/#:~:text=Among%20them%2C%2065%20sit%20on,the%20City%20of%20Montreal%2C%20including)) and know why some sources list a higher number. This ensures your data on Montreal’s municipal government is accurate and properly contextualized. 

