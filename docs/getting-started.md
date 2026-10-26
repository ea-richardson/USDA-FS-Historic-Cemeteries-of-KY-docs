---
title: Getting Started
nav_order: 3
---

# Getting Started
{: .warning }
When sources disagree, **don't erase the disagreement to simply make the dataset look clean.** Preserving evidence and documenting uncertainty during this phase is preferable to silently choosing one interpretation or attribute and losing the information that led to it.

## County Selection
**Define the County-Quadrangle Dictionary:**

*The countyquads dictionary was manually created using the [Kentucky Geological Survey's Topographic Map Index](https://kgs.uky.edu/kgsweb/download/topo/topoindex.pdf). The dictionary may contain errors due to manual entry. Always verify that the selected quadrangles cover the entire county.*

**Please note any obviously missing or unexpected quadrangles and let the senior intern or supervisor know.**

{: .note }
**Regarding topos:** Some quadrangles are named for places located in neighboring states rather than Kentucky. This is normal. In addition, Kentucky county boundaries do not correspond neatly to USGS quadrangle boundaries, and some quadrangles cross: county boundaries, state boundaries, or both.

```python
# Dictionary mapping Kentucky counties to their respective quadrangles
countyquads = {
    'Adair' : ['Campbellsville', 'Mannsville', 'Clementsville', 'Gresham', 'Cane Valley', 'Knifley', 'Dunnville', 'East Fork', 'Gradyville', 'Columbia', 'Montpelier', 'Russell Springs', 'Breeding', 'Amandaville', 'Creelsboro'],
    'Allen' : ['Polkville', 'Meador', 'Lucas', 'Drake', 'Allen Springs', 'Scottsville', 'Austin', 'Tracy', 'Hickory Flat', 'Adolphus', 'Petroleum', 'Holland', 'Fountain Run'],
    'Anderson' : ['Waddy', 'Frankfort West', 'Mount Eden', 'Glensboro', 'Lawrenceburg', 'Tyrone', 'Chaplin', 'Ashbrook', 'McBrayer', 'Salvisa'],
    'Ballard' : ['Olmsted, IL', 'Bandana', 'Cairo, IL', 'Barlow', 'La Center', 'Heath', 'Wyatt, MO', 'Wickliffe', 'Blandville', 'Lovelaceville'],
    'Barren' : ['Mammoth Cave', 'Horse Cave', 'Park', 'Center', 'Smiths Grove', 'Park City', 'Glasgow North', 'Hiseville', 'Sulphur Well', 'Meador', 'Lucas', 'Glasgow South', 'Temple Hill', 'Summer Shade', 'Austin', 'Tracy', 'Freedom', 'Sulphur Lick', 'Fountain Run'],
    'Bath' : ['Moorefield', 'Sherburne', 'Hillsboro', 'North Middletown', 'Sharpsburg', 'Owingsville', 'Colfax', 'Farmers', 'Preston', 'Olympia', 'Salt Lick', 'Bangor', 'Means', 'Frenchburg'],
    'Bell' : ['Scalf', 'Beverly', 'Helton', 'Artemus', 'Pineville', 'Balkan', 'Wallins Creek', 'Frakes', 'Kayjay', 'Middlesboro North', 'Varilla', 'Ewing', 'Eagan, TN', 'Mingo Mountains, TN', 'Middlesboro South, TN'],
    'Boone' : ['Hooven, OH', 'Addyston, OH', 'Aurora, IN', 'Lawrenceburg, IN', 'Burlington', 'Covington', 'Aberdeen, IN', 'Rising Sun', 'Union', 'Independence', 'Patriot', 'Verona', 'Walton'],
    'Bourbon' : ['Leesburg', 'Shawhan', 'Millersburg', 'Carlisle', 'Centerville', 'Paris West', 'Paris East', 'North Middletown', 'Sharpsburg', 'Lexington East', 'Clintonville', 'Austerlitz', 'Sideview'],
    'Boyd' : ['Ironton, OH', 'Argillite', 'Ashland', 'Catlettsburg, OH', 'Rush', 'Boltsfork', 'Burnaugh', 'Webbville', 'Fallsburg', 'Prichard, WV'],
    'Boyle' : ['Mackville', 'Perryville', 'Danville', 'Bryantsville', 'Gravel Switch', 'Parksville', 'Junction City', 'Stanford'],
    'Bracken' : ['Moscow, OH', 'Felicity, OH', 'Higginsport, OH', 'Berlin', 'Brooksville', 'Germantown', 'Claysville', 'Mount Olivet', 'Sardis'],
    'Breathitt' : ['Campton', 'Landsaw', 'Lee City', 'Seitz', 'Salyersville', 'Tallega', 'Jackson', 'Quicksand', 'Guage', 'Tiptop', 'David', 'Cowcreek', 'Canoe', 'Haddix', 'Noble', 'Vest', 'Handshoe', 'Mistletoe', 'Buckhorn', 'Krypton'],
    'Breckinridge' : ['Derby, IN', 'Alton', 'Rome, IN', 'Lodiburg', 'Irvington', 'Guston', 'Cloverport', 'Mattingly', 'Hardinsburg', 'Garfield', 'Big Spring', 'Fordsville', 'Glen Dean', 'Kingswood', 'Custer', 'Constantine', 'Olaton', 'Falls of Rough', 'McDaniels', 'Madrid'],
    'Bullitt' : ['Kosmosdale, IN', 'Valley Station', 'Brooks', 'Mount Washington', 'Waterford', 'Fort Knox', 'Pitts Point', 'Shepherdsville', 'Samuels', 'Fairfield', 'Colesburg', 'Lebanon Junction', 'Cravens'],
    'Butler' : ['Rosine', 'Spring Lick', 'Cromwell', 'Flener', 'Welchs Creek', 'Ready', 'Rochester', 'South Hill', 'Morgantown', 'Riverside', 'Reedyville', 'Dunmor', 'Sugar Grove', 'Hadley'],
    'Caldwell' : ['Providence', 'Marion', 'Shady Grove', 'Dalton', 'Fredonia', 'Crider', 'Olney', 'Dawson Springs', 'Eddyville', 'Princeton West', 'Princeton East', 'Dawson Springs SW', 'Lamasco', 'Cobb', 'Gracey'],
    'Calloway' : ['Kirksey', 'Dexter', 'Hico', 'Rushing Creek', 'Lynn Grove', 'Hazel', 'New Concord', 'Hamlin', 'Puryear, TN', 'Buchanan, TN', 'Paris Landing, TN'],
    'Campbell' : ['Covington', 'Newport', 'Withamsville, OH', 'Alexandria', 'New Richmond', 'Laurel, OH', 'De Mossville', 'Butler', 'Moscow, OH'],
    'Carlisle' : ['Wickliffe', 'Blandville', 'Lovelaceville', 'Wickliffe SW', 'Arlington', 'Milburn'],
    'Carroll' : ['Vevay North, IN', 'Madison East', 'Carrollton', 'Vevay South', 'Sanders', 'Campbellsburg', 'Worthville'],
    'Carter' : ['Garrison', 'Wesleyville', 'Tygarts Valley', 'Oldtown', 'Argillite', 'Soldier', 'Olive Hill', 'Grahn', 'Grayson', 'Rush', 'Haldeman', 'Ault', 'Brun', 'Willard', 'Webbville'],
    'Casey' : ['Gravel Switch', 'Parksville', 'Junction City', 'Bradfordsville', 'Bradfordsville NE', 'Ellisburg', 'Hustonville', 'Mannsville', 'Clementsville', 'Liberty', 'Yosemite', 'Eubank', 'Dunnville', 'Phil', 'Mintonville', 'Science Hill', 'Eli'],
    'Christian' : ['Dawson Springs', 'Saint Charles', 'Nortonville', 'Graham', 'Dawson Springs SW', 'Dawson Springs SE', 'Crofton', 'Haleys Mill', 'Gracey', 'Pleasant Green Hill', 'Kelly', 'Honey Grove', 'Caledonia', 'Church Hill', 'Hopkinsville', 'Pembroke', 'Roaring Spring', 'Herndon', 'Oak Grove', 'Hammacksville'],
    'Clark' : ['Clintonville', 'Austerlitz', 'Sideview', 'Ford', 'Winchester', 'Hedges', 'Levee', 'Union City', 'Palmer'],
    'Clay' : ['Tyner', 'Maulden', 'Oneida', 'Mistletoe', 'Portersburg', 'Manchester', 'Barcreek', 'Big Creek', 'Blackwater', 'Hima', 'Ogle', 'Creekville', 'Scalf', 'Beverly'],
    'Clinton' : ['Creelsboro', 'Wolf Creek Dam', 'Cumberland City', 'Frogue', 'Albany', 'Savage', 'Powersburg', 'Byrdstown, TN', 'Moodyville, TN', 'Pall Mall, TN'],
    'Crittenden' : ['Dekoven', 'Sturgis', 'Rosiclare, IL', 'Cave In Rock', 'Repton', 'Blackford', 'Providence', 'Lola', 'Salem', 'Marion', 'Shady Grove', 'Dalton', 'Dycusburg', 'Fredonia'],
    'Cumberland' : ['Breeding', 'Amandaville', 'Creelsboro', 'Dubre', 'Waterview', 'Burkesville', 'Wolf Creek Dam', 'Vernon', 'Blacks Ferry', 'Frogue', 'Albany', 'Dale Hollow Dam, TN', 'Dale Hollow Reservoir SE, TN'],
    'Daviess' : ['Yankeetown, IN', 'Rockport, IN', 'Lewisport', 'Reed', 'Owensboro West', 'Owensboro East', 'Maceo', 'Pellville', 'Delaware', 'Curdsville', 'Panther', 'Sutherland', 'Philpot', 'Whitesville', 'Calhoun', 'Glenville', 'Utica', 'Pleasant Ridge'],
    'Edmonson' : ['Ready', 'Bee Spring', 'Nolin Reservoir', 'Cub Run', 'Reedyville', 'Brownsville', 'Rhoda', 'Mammoth Cave', 'Bristow', 'Smiths Grove', 'Park City'],
    'Elliott' : ['Olive Hill', 'Haldeman', 'Ault', 'Bruin', 'Willard', 'Wrigley', 'Sandy Hook', 'Isonville', 'Mazie', 'Lenox', 'Dingus'],
    'Estill' : ['Palmer', 'Clay City', 'Stanton', 'Moberly', 'Panola', 'Irvine', 'Cobhill', 'Zachariah', 'Alcorn', 'Leighton', 'Heidelberg'],
    'Fayette' : ['Georgetown', 'Centerville', 'Paris West', 'Versailles', 'Lexington West', 'Lexington East', 'Clintonville', 'Keene', 'Nicholasville', 'Coletown', 'Ford', 'Valley View', 'Richmond North'],
    'Fleming' : ['Orangeburg', 'Tollesboro', 'Cowan', 'Elizaville', 'Flemingsburg', 'Burtonville', 'Stricklett', 'Moorefield', 'Sherburne', 'Hillsboro', 'Plummers Landing', 'Cranston', 'Colfax', 'Farmers'],
    'Floyd' : ['Paintsville', 'Ivyton', 'Prestonsburg', 'Lancer', 'Thomas', 'David', 'Martin', 'Harold', 'Broad Bottom', 'Handshoe', 'Wayland', 'McDowell', 'Pikeville', 'Kite', 'Wheelwright'],
    'Franklin' : ['Polsgrove', 'Switzer', 'Stamping Ground', 'Waddy', 'Frankfort West', 'Frankfort East', 'Midway', 'Lawrenceburg', 'Tyrone'],
    'Fulton' : ['Bayouville, MO', 'Wolf Island', 'Oakton', 'New Madrid SE, MO', 'Hubbard Lake', 'Bondurant', 'Hickman', 'Cayce', 'Crutchfield', 'Water Valley', 'Point Pleasant, MO', 'Tiptonville, TN'],
    'Gallatin' : ['Vevay North, IN', 'Florence, IN', 'Patriot', 'Verona', 'Vevay South', 'Sanders', 'Glencoe'],
    'Garrard' : ['Wilmore', 'Little Hickman', 'Bryantsville', 'Buckeye', 'Kirksville', 'Stanford', 'Lancaster', 'Paint Lick', 'Berea', 'Brodhead', 'Wildie'],
    'Grant' : ['Patriot', 'Verona', 'Walton', 'Glencoe', 'Elliston', 'Williamstown', 'Goforth', 'Owenton', 'Lawrenceville', 'Mason', 'Berry', 'New Columbus', 'Sadieville'],
    'Graves' : ['Lovelaceville', 'Melber', 'Symsonia', 'Elva', 'Fancy Farm', 'Hickory', 'Westplains', 'Oak Level', 'Dublin', 'Mayfield', 'Farmington', 'Kirksey', 'Water Valley', 'Cuba', 'Lynnville', 'Lynn Grove'],
    'Grayson' : ['Olaton', 'Falls of Rough', 'McDaniels', 'Madrid', 'Big Clifty', 'Summit', 'Rosine', 'Spring Lick', 'Caneyville', 'Leitchfield', 'Clarkson', 'Millerstown', 'Welchs Creek', 'Ready', 'Bee Spring', 'Nolin Reservoir', 'Cub Run'],
    'Green' : ['Magnolia', 'Hibernia', 'Saloma', 'Hudgins', 'Summersville', 'Greensburg', 'Center', 'Exie', 'Gresham', 'Cane Valley', 'Sulphur Well', 'East Fork', 'Gradyville'],
    'Greenup' : ['New Boston, OH', 'Friendship', 'Portsmouth', 'Wheelersburg, OH', 'Garrison', 'Brushart', 'Load', 'Greenup', 'Ironton, OH', 'Wesleyville', 'Tygarts Valley', 'Oldtown', 'Argillite', 'Ashland', 'Rush'],
    'Hancock' : ['Lewisport', 'Tell City', 'Cannelton, IN', 'Maceo', 'Pellville', 'Cloverport', 'Whitesville', 'Fordsville', 'Glen Dean'],
    'Hardin' : ['Kosmosdale, IN', 'Fort Knox', 'Pitts Point', 'Big Spring', 'Flaherty', 'Vine Grove', 'Colesburg', 'Lebanon Junction', 'Custer', 'Constantine', 'Howe Valley', 'Cecilia', 'Elizabethtown', 'Nelsonville', 'Madrid', 'Big Clifty', 'Summit', 'Sonora', 'Tonieville', 'Millerstown', 'Upton'],
    'Harlan' : ['Leatherwood', 'Tilford', 'Roxana', 'Helton', 'Bledsoe', 'Nolansburg', 'Louellen', 'Benham', 'Appalachia, VA', 'Balkan', 'Wallins Creek', 'Harlan', 'Evarts', 'Pennington Gap, VA', 'Keokee, VA', 'Varilla', 'Ewing', 'Rose Hill, VA', 'Hubbard Springs, VA'],
    'Harrison' : ['Mason', 'Berry', 'Kelat', 'Claysville', 'Sadieville', 'Breckinridge', 'Cynthiana', 'Shady Nook', 'Piqua', 'Leesburg', 'Shawhan', 'Millersburg'],
    'Hart' : ['Millerstown', 'Upton', 'Hammonville', 'Magnolia', 'Nolin Reservoir', 'Cub Run', 'Munfordville', 'Canmer', 'Hudgins', 'Mammoth Cave', 'Horse Cave', 'Park', 'Center'],
    'Henderson' : ['Mount Vernon, IN', 'Caborn, IN', 'West Franklin, IN', 'Evansville, IN', 'Newburgh, IN', 'Yankeetown, IN', 'Uniontown', 'Smith Mills', 'Wilson', 'Henderson', 'Spottsville', 'Reed', 'Waverly', 'Poole', 'Robards', 'Delaware', 'Curdsville'],
    'Henry' : ['Bedford', 'Campbellsburg', 'Worthville', 'New Liberty', 'Smithfield', 'New Castle', 'Franklinton', 'Gratz', 'Ballardsville', 'Eminence', 'North Pleasureville', 'Polsgrove', 'Switzer'],
    'Hickman' : ['Wickliffe SW', 'Arlington', 'Milburn', 'Fancy Farm', 'Wolf Island', 'Oakton', 'Clinton', 'Dublin', 'Cayce', 'Crutchfield', 'Water Valley'],
    'Hopkins' : ['Beech Grove', 'Calhoun', 'Providence', 'Nebo', 'Slaughters', 'Hanson', 'Sacramento', 'Dalton', 'Coiltown', 'Madisonville West', 'Madisonville East', 'Millport', 'Olney', 'Dawson Springs', 'Saint Charles', 'Nortonville', 'Graham' ,'Dawson Springs SE', 'Crofton'],
    'Jackson' : ['Bighill', 'Alcorn', 'Leighton', 'Johnetta', 'Sandgap', 'McKee', 'Sturgeon', 'Livingston', 'Parrot', 'Tyner', 'Maulden'],
    'Jefferson' : ['Charlestown, IN', 'New Albany, IN', 'Jeffersonville, IN', 'Anchorage', 'Crestwood', 'Lanesville, IN', 'Louisville West', 'Louisville East', 'Jeffersontown', 'Fisherville', 'Kosmosdale, IN', 'Valley Station', 'Brooks', 'Mount Washington', 'Waterford', 'Fort Knox'],
    'Jessamine' : ['Versailles', 'Keene', 'Nicholasville', 'Coletown', 'Wilmore', 'Little Hickman', 'Valley View', 'Buckeye'],
    'Johnson' : ['Mazie', 'Redbush', 'Sitka', 'Richardson', 'Milo', 'Salyersville North', 'Oil Springs', 'Paintsville', 'Offutt', 'Inez', 'Ivyton', 'Prestonsburg', 'Lancer'],
    'Kenton' : ['Covington', 'Newport', 'Independence', 'Alexandria', 'Walton', 'De Mossville'],
    'Knott' : ['Tiptop', 'David', 'Noble', 'Vest', 'Handshoe', 'Wayland', 'Hazard North', 'Carrie', 'Hindman', 'Kite', 'Wheelwright', 'Hazard South', 'Vicco', 'Blackey', 'Mayking'],
    'Knox' : ['Blackwater', 'Hima', 'Ogle', 'Corbin', 'Heidrick', 'Fount', 'Scalf', 'Beverly', 'Rockholds', 'Barbourville', 'Artemus', 'Pineville', 'Frakes', 'Kayjay'],
    'LaRue' : ['Elizabethtown', 'Nelsonville', 'New Haven', 'Sonora', 'Tonieville', 'Hodgenville', 'Howardstown', 'Raywick', 'Upton', 'Hammonville', 'Magnolia', 'Hibernia', 'Saloma'],
    'Laurel' : ['Livingston', 'Parrot', 'Tyner', 'Billows', 'Bernstadt', 'London', 'Portersburg', 'Ano', 'London SW', 'Lily', 'Blackwater', 'Hima', 'Sawyer', 'Vox', 'Corbin', 'Heidrick'],
    'Lawrence' : ['Boltsfork', 'Burnaugh', 'Willard', 'Webbville', 'Fallsburg', 'Prichard, WV', 'Isonville', 'Mazie', 'Blaine', 'Adams', 'Louisa', 'Dingus', 'Redbush', 'Sitka', 'Richardson', 'Milo', 'Webb, WV'],
    'Lee' : ['Cobhill', 'Zachariah', 'Campton', 'Leighton', 'Heidelberg', 'Beattyville', 'Tallega', 'Sturgeon', 'Booneville'],
    'Leslie' : ['Buckhorn', 'Krypton', 'Big Creek', 'Hyden West', 'Hyden East', 'Hazard South', 'Creekville', 'Hoskinston', 'Cutshin', 'Leatherwood', 'Beverly', 'Helton', 'Beldsoe', 'Nolansburg'],
    'Letcher' : ['Kite', 'Wheelwright', 'Vicco', 'Blackey', 'Mayking', 'Jenkins West', 'Jenkins East', 'Tilford', 'Roxana', 'Whitesburg', 'Flat Gap, VA', 'Nolansburg', 'Louellen', 'Benham', 'Appalachia, VA'],
    'Lewis' : ['Maysville East, OH', 'Machester Islands', 'Concord, OH', 'Buena Vista, OH', 'Pond Run, OH', 'Friendship', 'Orangeburg', 'Tollesboro', 'Charters', 'Vanceburg', 'Garrison', 'Brushart', 'Burtonville', 'Stricklett', 'Head of Grassy', 'Wesleyville', 'Cranston', 'Soldier', 'Olive Hill'],
    'Lincoln' : ['Bryantsville', 'Junction City', 'Stanford', 'Lancaster', 'Paint Lick', 'Hustonville', 'Halls Gap', 'Crab Orchard', 'Brodhead', 'Yosemite', 'Eubank', 'Woodstock', 'Maretburg', 'Science Hill'],
    'Livingston' : ['Shetlerville, IL', 'Rosiclare, IL', 'Brownfield, IL', 'Golconda', 'Lola', 'Salem', 'Smithland', 'Burna', 'Dycusburg', 'Paducah East', 'Little Cypress', 'Calvert City', 'Grand Rivers', 'Briensburg', 'Birmingham Point'],
    'Logan' : ['Rosewood', 'Dunmor', 'Quality', 'Sugar Grove', 'Sharon Grove', 'Lewisburg', 'Homer', 'South Union', 'Rockfield', 'Olmstead', 'Russellville', 'Dennis', 'Auburn', 'Allensville', 'Dot', 'Adairville', 'Prices Mill'],
    'Lyon' : ['Dycusburg', 'Fredonia', 'Grand Rivers', 'Eddyville', 'Princeton West', 'Birmingham Point', 'Mont', 'Lamasco', 'Fairdealing'],
    'Madison' : ['Coletown', 'Ford', 'Winchester', 'Little Hickman', 'Valley View', 'Richmond North', 'Union City', 'Palmer', 'Buckeye', 'Kirksville', 'Richmond South', 'Moberly', 'Panola', 'Paint Lick', 'Berea', 'Bighill', 'Alcorn'],
    'Magoffin' : ['Lenox', 'Dingus', 'Cannel City', 'White Oak', 'Salyersville North', 'Oil Springs', 'Lee City', 'Seitz', 'Salyersville South', 'Ivyton', 'Guage', 'Tiptop', 'David', 'Handshoe'],
    'Marion' : ['Loretto', 'Saint Catharine', 'Springfield', 'Mackville', 'Howardstown', 'Raywick', 'Lebanon West', 'Lebanon East', 'Gravel Switch', 'Saloma', 'Spurlington', 'Bradfordsville', 'Bradfordsville NE'],
    'Marshall' : ['Little Cypress', 'Calvert City', 'Elva', 'Briensburg', 'Birmingham Point', 'Oak Level', 'Hardin', 'Fairdealing', 'Fenton', 'Kirksey', 'Dexter', 'Hico', 'Rushing Creek'],
    'Martin' : ['Milo', 'Webb, WV', 'Offutt', 'Inez', 'Kermit', 'Naugatuck, WV', 'Lancer', 'Thomas', 'Varney', 'Williamson'],
    'Mason' : ['Higginsport, OH', 'Russellville, OH', 'Germantown', 'Maysville West', 'Maysville East, OH', 'Sardis', 'Mays Lick', 'Orangeburg', 'Tollesboro', 'Cowan', 'Elizaville', 'Flemingsburg'],
    'McCracken' : ['Bandana', 'Joppa, IL', 'Metropolis, IL', 'La Center', 'Heath', 'Paducah West', 'Paducah East', 'Little Cypress', 'Lovelaceville', 'Melber', 'Symsonia', 'Elva'],
    'McCreary' : ['Burnside', 'Hail', 'Sawyer', 'Coopersville', 'Nevelsville', 'Wiborg', 'Cumberland Falls', 'Parmleysville', 'Bell Farm', 'Barthell', 'Whitley City', 'Hollyhill', 'Sharp Place, TN', 'Barthell SW, TN', 'Oneida North, TN', 'Winfield, TN', 'Ketchen, TN', 'Jellico West, TN'],
    'McLean' : ['Delaware', 'Curdsville', 'Beech Grove', 'Calhoun', 'Glenville', 'Utica', 'Sacramento', 'Livermore', 'Equality'],
    'Meade' : ['Beechwood, IN', 'Leavenworth, IN', 'Alton', 'New Amsterdam', 'Mauckport, IN', 'Laconia, IN', 'Lodiburg', 'Irvington', 'Guston', 'Rock Haven', 'Fort Knox', 'Big Spring', 'Flaherty', 'Vine Grove'],
    'Menifee' : ['Olympia', 'Salt Lick', 'Bangor', 'Means', 'Frenchburg', 'Scranton', 'Ezel', 'Slade', 'Pomeroyton', 'Hazel Green'],
    'Mercer' : ['Ashbrook', 'McBrayer', 'Salvisa', 'Cardwell', 'Cornishville', 'Harrodsburg', 'Wilmore', 'Mackville', 'Perryville', 'Danville', 'Bryantsville'],
    'Metcalfe' : ['Park', 'Center', 'Hiseville', 'Sulphur Well', 'East Fork', 'Gradyville', 'Summer Shade', 'Edmonton', 'Breeding', 'Sulphur Lick', 'Dubre', 'Waterview'],
    'Monroe' : ['Tracy', 'Freedom', 'Sulphur Lick', 'Dubre', 'Fountain Run', 'Gamaliel', 'Tompkinsville', 'Vernon', 'Blacks Ferry', 'Galen, TN', 'Red Boiling Springs, TN', 'Union Hill, TN', 'Celina, TN', 'Dale Hollow Dam, TN'],
    'Montgomery' : ['North Middletown', 'Sharpsburg', 'Owingsville', 'Sideview', 'Mount Sterling', 'Preston', 'Hedges', 'Levee', 'Means'],
    'Morgan' : ['Bangor', 'Wrigley', 'Sandy Hook', 'Isonville', 'Ezel', 'West Liberty', 'Lenox', 'Dingus', 'Redbush', 'Pomeroyton', 'Hazel Green', 'Cannel City', 'White Oak', 'Salyersville North', 'Oil Springs', 'Lee City', 'Seitz'],
    'Muhlenberg' : ['Sacramento', 'Livermore', 'Equality', 'Madisonville East', 'Millport', 'Central City West', 'Central City East', 'Paradise', 'Graham', 'Greenville', 'Drakesboro', 'Rochester', 'Haleys Mill', 'Kirkmansville', 'Rosewood', 'Dunmor'],
    'Nelson' : ['Samuels', 'Fairfield', 'Bloomfield', 'Chaplin', 'Lebanon Junction', 'Cravens', 'Bardstown', 'Maud', 'Brush Grove', 'Nelsonville', 'New Haven', 'Loretto', 'Saint Catharine', 'Hodgenville', 'Howardstown', 'Raywick'],
    'Nicholas' : ['Shady Nook', 'Piqua', 'Cowan', 'Millersburg', 'Carlisle', 'Moorefield', 'Sherburne', 'North Middletown', 'Sharpsburg'],
    'Ohio' : ['Philpot', 'Whitesville', 'Fordsville', 'Utica', 'Pleasant Ridge', 'Dundee', 'Olaton', 'Falls of Rough', 'Livermore', 'Equality', 'Hartford', 'Horton', 'Rosine', 'Spring Lick', 'Central City West', 'Central City East', 'Paradise', 'Cromwell', 'Flener', 'Rochester', 'South Hill'],
    'Oldham' : ['Bethlehem, IN', 'Bedford', 'Charlestown, IN', 'Owen', 'La Grange', 'Smithfield', 'Jeffersonville, IN', 'Anchorage', 'Crestwood', 'Ballardsville'],
    'Owen' : ['Vevay South', 'Sanders', 'Glencoe', 'Worthville', 'New Liberty', 'Owenton', 'Lawrenceville', 'Gratz', 'Monterey', 'New Columbus', 'Sadieville', 'Polsgrove', 'Switzer', 'Stamping Ground'],
    'Owsley' : ['Leighton', 'Heidelberg', 'Beattyville', 'Tallega', 'McKee', 'Sturgeon', 'Booneville', 'Cowcreek', 'Maulden', 'Oneida', 'Mistletoe'],
    'Pendleton' : ['Walton', 'De Mossville', 'Butler', 'Moscow, OH', 'Williamstown', 'Goforth', 'Falmouth', 'Berlin', 'Berry', 'Kelat', 'Claysville'],
    'Perry' : ['Canoe', 'Haddix', 'Noble', 'Vest', 'Mistletoe', 'Buckhorn', 'Krypton', 'Hazard North', 'Carrie', 'Big Creek', 'Hyden West', 'Hyden East', 'Hazard South', 'Vicco', 'Leatherwood', 'Tilford', 'Louellen'],
    'Pike' : ['Thomas', 'Varney', 'Williamson', 'Delbarton, WV', 'Broad Bottom', 'Meta', 'Belfry', 'Matewan', 'Majestic, WV', 'Wharncliffe, WV', 'McDowell', 'Pikeville', 'Millard', 'Lick Creek', 'Jamboree', 'Hurley, VA', 'Wheelwright', 'Dorton', 'Hellier', 'Elkhorn City', 'Harman, VA', 'Jenkins West', 'Jenkins East', 'Clintwood'],
    'Powell' : ['Hedges', 'Levee', 'Means', 'Frenchburg', 'Palmer', 'Clay City', 'Stanton', 'Slade', 'Cobhill', 'Zachariah'],
    'Pulaski' : ['Eubank', 'Woodstock', 'Maretburg', 'Phil', 'Mintonville', 'Science Hill', 'Bobtown', 'Shopville', 'Billows', 'Eli', 'Faubush', 'Delmer', 'Somerset', 'Dykes', 'Ano', 'Mill Springs', 'Frazer', 'Burnside', 'Hail', 'Sawyer', 'Nevelsville'],
    'Robertson' : ['Claysville', 'Mount Olivet', 'Sardis', 'Shady Nook', 'Piqua', 'Cowan'],
    'Rockcastle' : ['Berea', 'Bighill', 'Brodhead', 'Wildie', 'Johnetta', 'Woodstock', 'Maretburg', 'Mount Vernon', 'Livingston', 'Shopville', 'Billows', 'Bernstadt'],
    'Rowan' : ['Stricklett', 'Plummers Landing', 'Cranston', 'Soldier', 'Colfax', 'Farmers', 'Morehead', 'Haldeman', 'Ault', 'Salt Lick', 'Bangor', 'Wrigley'],
    'Russell' : ['Dunnville', 'Phil', 'Montpelier', 'Russell Springs', 'Eli', 'Faubush', 'Amandaville', 'Creelsboro', 'Jamestown', 'Jabez', 'Mill Springs', 'Wolf Breek Dam', 'Cumberland City'],
    'Scott' : ['New Columbus', 'Sadieville', 'Breckinridge', 'Stamping Ground', 'Delaplain', 'Leesburg', 'Midway', 'Georgetown', 'Centerville', 'Versailles', 'Lexington West'],
    'Shelby' : ['Crestwood', 'Ballardsville', 'Eminence', 'North Pleasureville', 'Polsgrove', 'Fisherville', 'Simpsonville', 'Shelbyville', 'Waddy', 'Taylorsville', 'Mount Eden', 'Glensboro'],
    'Simpson' : ['South Union', 'Rockfield', 'Auburn', 'Woodburn', 'Drake', 'Adairville', 'Prices Mill', 'Franklin', 'Hickory Flat'],
    'Spencer' : ['Fisherville', 'Simpsonville', 'Mount Washington', 'Waterford', 'Taylorsville', 'Mount Eden', 'Glensboro', 'Samuels', 'Fairfield', 'Bloomfield', 'Chaplin'],
    'Taylor' : ['Hibernia', 'Saloma', 'Spurlington', 'Bradfordsville', 'Bradfordsville NE', 'Greensburg', 'Campbellsville', 'Mannsville', 'Gresham', 'Cane Valley'],
    'Todd' : ['Haleys Mill', 'Kirkmansville', 'Rosewood', 'Honey Grove', 'Allegre', 'Sharon Grove', 'Pembroke', 'Elkton', 'Olmstead', 'Hammacksville', 'Guthrie', 'Allensville'],
    'Trigg' : ['Dawson Springs', 'Birmingham Point', 'Mont', 'Lamasco', 'Cobb', 'Gracey', 'Fairdealing', 'Fenton', 'Canton', 'Cadiz', 'Caledonia', 'Rushing Creek', 'Model', 'Johnson Hollow', 'Roaring Spring'],
    'Trimble' : ['Madison West, IN', 'Madison East', 'Carrollton', 'Bethlehem, IN', 'Bedford', 'Campbellsburg', 'Smithfield'],
    'Union' : ['Mount Vernon, IN', 'Wabash Island, IL', 'Uniontown', 'Smith Mills', 'Shawneetown, IL', 'Grove Center', 'Morganfield', 'Waverly', 'Poole', 'Saline Mines, IL', 'Dekoven', 'Sturgis', 'Bordley', 'Blackford'],
    'Warren' : ['Morgantown', 'Riverside', 'Reedyville', 'Brownsville', 'Sugar Grove', 'Hadley', 'Bowling Green North', 'Bristow', 'Smiths Grove', 'Park City', 'South Union', 'Rockfield', 'Bowling Green South', 'Polkville', 'Meador', 'Woodburn', 'Drake', 'Allen Springs'],
    'Washington' : ['Chaplin', 'Ashbrook', 'Bardstown', 'Maud', 'Brush Grove', 'Cardwell', 'Loretto', 'Saint Catharine', 'Springfield', 'Mackville'],
    'Wayne' : ['Jamestown', 'Jabez', 'Mill Springs', 'Frazer', 'Burnside', 'Cumberland City', 'Parnell', 'Monticello', 'Coopersville', 'Nevelsville', 'Savage', 'Powersburg', 'Parmleysville', 'Bell Farm', 'Pall Mall, TN', 'Sharp Place, TN'],
    'Webster' : ['Poole', 'Robards', 'Delaware', 'Sturgis', 'Bordley', 'Dixon', 'Sebree', 'Beech Grove', 'Blackford', 'Providence', 'Nebo', 'Slaughters', 'Hanson', 'Dalton'],
    'Whitley' : ['Sawyer', 'Vox', 'Corbin', 'Cumberland Falls', 'Wofford', 'Rockholds', 'Barbourville', 'Hollyhill', 'Williamsburg', 'Saxton', 'Frakes', 'Ketchen, TN', 'Jellico West, TN', 'Jellico East, TN', 'Eagan, TN'],
    'Wolfe' : ['Slade', 'Pomeroyton', 'Hazel Green', 'Cannel City', 'Zachariah', 'Campton', 'Landsaw', 'Lee City', 'Seitz', 'Jackson'],
    'Woodford' : ['Frankfort East', 'Midway', 'Tyrone', 'Versailles', 'Salvisa', 'Keene', 'Harrodsburg', 'Wilmore']
    }
```

**Create a "county" variable:**

```python
while True:
    county = input("What county are you working on? (Press enter to exit): ")
    
    if county == "":
        print("Terminating script.")
        break
    elif county in countyquads:
        print(f"You have selected \033[1m{county}\033[0m County.")
        break
    else:
        print("The county you entered is not in the dictionary. Please try again.")
```

## Directory Setup
**Create a new directory structure, either inside an existing "topos" directory or within a newly created "topos" directory, and format the subdirectory names based on the current date and county name:**

*How It Works:*
- *Prompts the user to decide if an existing "topos" directory should be used or a new one should be created.*
- *If an existing "topos" directory is chosen, it generates a subdirectory with a formatted name based on the county name, current date, and the next available number.*
- *If a new "topos" directory is created, the script sets up the "topos" directory first and then creates the same county subdirectory with the appropriate naming convention.*

```python
import os
import tkinter as tk
from tkinter import filedialog
from datetime import datetime

def format_dir_name(name):
    """Format directory names by replacing spaces with hyphens and commas with underscores."""
    name = name.lower().replace(', ', '_').replace(' ', '-')
    name = name.replace('-_', '_') # Ensure no mixed characters like "-_"
    return name

def get_next_directory_number(base_directory):
    """Determine the next available directory number in the given base directory."""
    existing_dirs = os.listdir(base_directory)
    existing_numbers = []

    # Loop through existing directories and find those starting with a number
    for directory in existing_dirs:
        # Check if the directory starts with a number and a hyphen (e.g., "01-whitley")
        if directory[0].isdigit() and '-' in directory:
            # Extract the number from the beginning of the directory name
            number_part = directory.split('-')[0]
            if number_part.isdigit():
                existing_numbers.append(int(number_part))
    
    # Find the next available number (starting from 1)
    next_number = 1
    if existing_numbers:
        next_number = max(existing_numbers) + 1

    # Format the number as two digits (01, 02, etc.)
    return f"{next_number:02d}"

def get_current_date():
    """Return the current date formatted as MMDDYYYY."""
    return datetime.now().strftime("%m%d%Y")

# Initialize GUI file dialog
root = tk.Tk()
root.withdraw()

directory_order = input('Do you have an existing "topos" directory? (y/n) ').lower()

current_date = get_current_date()

if directory_order == "y":
    print(f'Select the existing "topos" directory.')
    topo_directory = filedialog.askdirectory()
    
    # Format the county name
    formatted_county = format_dir_name(county)
    
    # Get the next available number for the county directory
    next_number = get_next_directory_number(topo_directory)
    new_county_directory_name = f"{next_number}-{formatted_county}_{current_date}"
    county_directory = os.path.join(topo_directory, new_county_directory_name)
    
    # Create the county subdirectory
    os.makedirs(county_directory, exist_ok=True)
    print(f'Success! Created: "{os.path.normpath(county_directory)}')

elif directory_order == "n":
    print(f'Select the parent directory to create a new "topos" directory.')
    directory_path = filedialog.askdirectory()

    # Create "topos" directory
    topo_directory = os.path.join(directory_path, "topos")
    os.makedirs(topo_directory, exist_ok=True)
    print(f'Subdirectory "topos" created at: {os.path.normpath(topo_directory)}.')
    
    # Format the county name
    formatted_county = format_dir_name(county)
    
    # Get the next available number for the county directory
    next_number = get_next_directory_number(topo_directory)
    new_county_directory_name = f"{next_number}-{formatted_county}_{current_date}"
    county_directory = os.path.join(topo_directory, new_county_directory_name)
    
    # Create the county subdirectory
    os.makedirs(county_directory, exist_ok=True)
    print(f'Subdirectory "{os.path.normpath(county_directory)}" created.')

else:
    print("Invalid input. Please re-run and enter 'y' or 'n'.")
```

**Connect the "topos" folder to the project:**

```python
import arcpy
import os

# Get the path of the currently open ArcGIS Pro project (if there is one open)
aprx = arcpy.mp.ArcGISProject("CURRENT")  # 'CURRENT' refers to the active project

# Check if the project is open
if not aprx:
    print("No project is currently open.")
else:
    # Get the current folder connections
    folder_connections = aprx.folderConnections

    # Define the new folder path using os.path.normpath
    new_folder = os.path.normpath(topo_directory)  # Normalize the path
    alias = "Topos Folder"  # Alias for the new folder connection
    is_home_folder = False  # Set to True if this should be the default home folder

    # Create a dictionary for the new folder connection
    new_folder_dict = {
        "connectionString": new_folder,
        "alias": alias,
        "isHomeFolder": is_home_folder
    }

    # Add the new folder connection to the list
    folder_connections.append(new_folder_dict)

    # Optional: Validate the folder path (ensure the folder exists if validate=True)
    validate = True

    # Update the folder connections in the project
    aprx.updateFolderConnections(folder_connections, validate)

    # Save the project after the update
    aprx.save()

    print("Folder connection updated successfully!")
```

## Download Topographic Maps
This project uses U.S. Geological Survey topographic maps from the 1950s (1:24,000 scale). If 1950s maps are not available, late 1940s or early 1960s maps may be substituted. Users have the option to manually download GeoTIFFs from either Esri's [Historical Topo Map Explorer](https://livingatlas.arcgis.com/topomapexplorer) or USGS's [topoView](https://ngmdb.usgs.gov/topoview/viewer).

Esri's source allows for direct download of GeoTIFFs, whereas downloads from the USGS tool must be extracted from a zipped folder before use. For this reason, the script below utilizes Esri Explorer.

**It may also be useful to download current topographic maps from USGS's topoView.** Newer maps may include cemetery names that were absent on older maps, especially those that have expanded considerably since the 1950s. Additionally, some cemeteries may no longer exist due to changes in the landscape, such as the creation of reservoirs. Although these changes can be observed on Google Maps or through the [KyFromAbove Program imagery](https://kygeonet.maps.arcgis.com/home/webmap/viewer.html?webmap=ba05e691cf3a4acd9583b12ccf09856e), in some cases, comparing topographic layers in ArcGIS Pro by toggling layers on and off is more effective.

**Generate URLs to access topographic maps for the quadrangles that cover the county of interest:**

*How It Works:*
- *Most quadrangles are named after a prominent place within the region.*
- *The script checks if the county is present in a predefined dictionary and retrieves the corresponding quadrangle names.*
- *For each quadrangle, the script queries the Nominatim API for geographic coordinates and generates an Esri map URL for the 1:24,000 scale topographic maps.*
- *It outputs the map URLs with formatting and provides instructions for downloading the maps.*

```python
import urllib.parse
import urllib.request
import urllib.error
import json
import time


# ============================================================
# TEXT FORMATTING
# ============================================================

RED = '\033[91m'
BOLD = '\033[1m'
UNDERLINE = '\033[4m'
RESET = '\033[0m'


# ============================================================
# NOMINATIM SETTINGS
# ============================================================

# Identifies this application to the Nominatim service.
# No personal email address is included.
USER_AGENT = "KentuckyTopoMapResearch/1.0"

# Nominatim search API
NOMINATIM_URL = "https://nominatim.openstreetmap.org/search?"

# Wait at least one second between requests to the
# public Nominatim service.
REQUEST_DELAY = 1.0

# Store coordinates already found during this notebook
# session so the same place does not need to be queried again.
coordinate_cache = {}


# ============================================================
# FUNCTION: GET COORDINATES
# ============================================================

def get_coordinates(place_name):
    """
    Get latitude and longitude for a Kentucky quadrangle
    using the Nominatim OpenStreetMap geocoding service.
    """

    # --------------------------------------------------------
    # Make sure the search is restricted to Kentucky
    # --------------------------------------------------------

    if ", KY" not in place_name.upper():
        search_name = f"{place_name}, KY"
    else:
        search_name = place_name

    # --------------------------------------------------------
    # Check whether this location has already been geocoded
    # --------------------------------------------------------

    if search_name in coordinate_cache:
        return coordinate_cache[search_name]

    # --------------------------------------------------------
    # Nominatim search parameters
    # --------------------------------------------------------

    params = {
        "q": search_name,
        "format": "jsonv2",
        "addressdetails": 1,
        "limit": 1,
        "countrycodes": "us"
    }

    # Construct the complete URL
    url = NOMINATIM_URL + urllib.parse.urlencode(params)

    # --------------------------------------------------------
    # Create request with identifying User-Agent
    # --------------------------------------------------------

    request = urllib.request.Request(
        url,
        headers={
            "User-Agent": USER_AGENT
        }
    )

    # --------------------------------------------------------
    # Send request to Nominatim
    # --------------------------------------------------------

    try:

        with urllib.request.urlopen(
            request,
            timeout=30
        ) as response:

            data = json.load(response)

        # ----------------------------------------------------
        # Process successful response
        # ----------------------------------------------------

        if data:

            lat = data[0]["lat"]
            lon = data[0]["lon"]

            # Save result in cache
            coordinate_cache[search_name] = (lat, lon)

            return lat, lon

        else:

            print(
                f"{RED}"
                f"No coordinates found for: "
                f"{search_name}"
                f"{RESET}"
            )

            coordinate_cache[search_name] = (None, None)

            return None, None

    # --------------------------------------------------------
    # Handle HTTP errors such as 403
    # --------------------------------------------------------

    except urllib.error.HTTPError as e:

        print(
            f"{RED}"
            f"Nominatim HTTP error for "
            f"'{search_name}': "
            f"{e.code} {e.reason}"
            f"{RESET}"
        )

        # Attempt to display the server's response
        try:

            error_message = e.read().decode(
                "utf-8",
                errors="replace"
            )

            if error_message:

                print(
                    f"Server response: "
                    f"{error_message[:500]}"
                )

        except Exception:
            pass

        return None, None

    # --------------------------------------------------------
    # Handle network errors
    # --------------------------------------------------------

    except urllib.error.URLError as e:

        print(
            f"{RED}"
            f"Network error for "
            f"'{search_name}': "
            f"{e.reason}"
            f"{RESET}"
        )

        return None, None

    # --------------------------------------------------------
    # Handle unexpected errors
    # --------------------------------------------------------

    except Exception as e:

        print(
            f"{RED}"
            f"Unexpected error for "
            f"'{search_name}': "
            f"{e}"
            f"{RESET}"
        )

        return None, None


# ============================================================
# FUNCTION: GENERATE ESRI MAP LINK
# ============================================================

def generate_map_link(lat, lon):
    """
    Generate an Esri Topographic Map Explorer URL
    centered on the supplied coordinates.
    """

    esri_url = (
        f"https://livingatlas.arcgis.com/topomapexplorer/"
        f"#maps=&loc={lon},{lat}&LoD=15.76"
    )

    return esri_url


# ============================================================
# MAIN PROCESS
# ============================================================

# Check whether the county exists in the countyquads dictionary

if county in countyquads.keys():

    quads = countyquads[county]

    # --------------------------------------------------------
    # Introductory message
    # --------------------------------------------------------

    print(
        f"{RED}"
        f"Please open the URL for each quadrangle, "
        f"filter for the 24k (1:24,000) scale, and "
        f"download the appropriate topographic map "
        f"for the year you want (~1950s)."
        f"{RESET}"
    )

    print(
        f"{BOLD}"
        f"{county} County requires "
        f"{UNDERLINE}{len(quads)}{RESET}{BOLD} "
        f"topographic maps:"
        f"{RESET}"
    )

    print()

    # --------------------------------------------------------
    # Process each quadrangle
    # --------------------------------------------------------

    for i, quad in enumerate(quads, start=1):

        print(
            f"{BOLD}"
            f"[{i}/{len(quads)}] {quad}"
            f"{RESET}"
        )

        # Get coordinates from Nominatim
        lat, lon = get_coordinates(quad)

        # ----------------------------------------------------
        # Coordinates successfully found
        # ----------------------------------------------------

        if lat is not None and lon is not None:

            # Generate Esri Topo Map Explorer URL
            esri_url = generate_map_link(lat, lon)

            print(
                f"    {esri_url}"
            )

        # ----------------------------------------------------
        # Coordinates not found
        # ----------------------------------------------------

        else:

            print(
                f"    {RED}"
                f"Coordinates not found. "
                f"Please manually search for "
                f"'{quad}' in the Esri Topo Map Explorer."
                f"{RESET}"
            )

        # ----------------------------------------------------
        # Wait between requests
        #
        # Do not wait after the final quadrangle.
        # ----------------------------------------------------

        if i < len(quads):

            time.sleep(REQUEST_DELAY)

    print()

    print(
        f"{BOLD}"
        f"Finished processing {len(quads)} quadrangles."
        f"{RESET}"
    )

# ============================================================
# COUNTY NOT FOUND
# ============================================================

else:

    print(
        f"{RED}"
        f"County not found in the predefined list.\n"
        f"Please make sure the county name is correct."
        f"{RESET}"
    )
```

## ArcGIS Pro Project Workspace
### Add *HistoricalCemeteries_KY* Geodatabase
**Select and add the *HistoricalCemeteries_KY* geodatabase to the project:**

*How It Works:*
- *The user selects a geodatabase folder via a file dialog using Tkinter.*
- *The script checks if the selected folder is a valid geodatabase and if the project is not read-only.*
- *The selected geodatabase is added to the project, the databases list is updated, and the project is saved.*

```python
import arcpy
import os
import tkinter as tk
from tkinter import filedialog

# Initialize Tkinter window (for file dialog)
root = tk.Tk()
root.withdraw()  # Hides the root window

# Prompt user to select a geodatabase
gdb_path = filedialog.askdirectory(title="Select HistoricalCemeteries_KY.gdb")

# Ensure the path is a valid geodatabase
if gdb_path and os.path.isdir(gdb_path) and gdb_path.lower().endswith('.gdb'):
    # Get the active ArcGIS Pro project
    aprx = arcpy.mp.ArcGISProject("CURRENT")  # Refers to the active project
    
    # Check if the project is read-only
    if aprx.isReadOnly:
        print('WARNING: Project is read-only. Exiting.')
        exit()
    
    # Get the current list of databases in the project
    db = aprx.databases
    
    # Append the selected geodatabase to the project (not as default)
    db.append({'databasePath': gdb_path, 'isDefaultDatabase': False})
    
    # Update the project's databases list
    aprx.updateDatabases(db, True)
    
    # Save the project after making the changes
    aprx.save()
    
    print(f"Geodatabase '{gdb_path}' added to the project.")
else:
    print("Invalid selection or no geodatabase selected. Exiting.")
```

**Make *HistoricalCemeteries_KY.gdb* the default database:**

```python
import arcpy

# Get the active ArcGIS project
aprx = arcpy.mp.ArcGISProject("CURRENT")

# Set the default geodatabase
aprx.defaultGeodatabase = gdb_path

# Save the project to apply the change
aprx.save()

# Print confirmation
print(f"Default geodatabase set to: {gdb_path}")
```

### Create and/or Rename a Map
**Rename an existing map or create a new map:**

*How It Works:*
- *The script lists all existing maps in the current ArcGIS Pro project, allowing the user to choose either to rename an existing map or create a new one.*
- *If creating a new map, the script checks for any naming conflicts and prompts the user to provide a new name if needed.*
- *The selected map is renamed based on a predefined county variable.*

```python
import arcpy

# Function to list all maps
def list_all_maps():
    try:
        aprx = arcpy.mp.ArcGISProject("CURRENT")
        maps = aprx.listMaps()
        return maps
    except Exception as e:
        arcpy.AddMessage(f"Error: {str(e)}")
        return []

# Function to rename an existing map
def rename_map(map_obj, new_name):
    try:
        map_obj.name = new_name
        arcpy.AddMessage(f"Map renamed to {new_name}")
    except Exception as e:
        arcpy.AddMessage(f"Error renaming map: {str(e)}")

# Function to create a new map
def create_new_map(aprx):
    # Check if a map with the same name exists
    maps = aprx.listMaps()
    new_map_name = f"{county}_County"
    
    # Check for duplicate names and prompt user if necessary
    existing_map_names = [map_obj.name for map_obj in maps]
    if new_map_name in existing_map_names:
        user_choice = input(f"A map with the name '{new_map_name}' already exists. Would you like to rename the new map? (y/n): ")
        if user_choice.lower() == "y":
            new_map_name = input("Enter a new name for the new map: ")
        else:
            arcpy.AddMessage("New map creation canceled.")
            return None
    
    try:
        new_map = aprx.createMap("New Map")
        # Rename the new map to the determined name
        rename_map(new_map, new_map_name)
        arcpy.AddMessage(f"New map created and renamed: {new_map.name}")
        return new_map
    except Exception as e:
        arcpy.AddMessage(f"Error creating new map: {str(e)}")
        return None

# Function to handle the renaming or creation process
def rename_or_create_map():
    aprx = arcpy.mp.ArcGISProject("CURRENT")
    maps = list_all_maps()

    # List all existing maps
    if len(maps) > 0:
        print("The following maps exist:")
        for idx, map_obj in enumerate(maps):
            print(f"{idx + 1}. {map_obj.name}")
    
    # Always ask the user if they want to create a new map
    user_choice = input(f"Which map would you like to rename (1-{len(maps)})? Or, enter '0' to create a new map: ")

    if user_choice == '0':
        # Create a new map and rename it to county_County
        return create_new_map(aprx), True
    else:
        try:
            selected_map = maps[int(user_choice) - 1]
            rename_map(selected_map, f"{county}_County")
            return selected_map, True
        except (ValueError, IndexError):
            arcpy.AddMessage("Invalid input, no map selected.")
            return None, False

# Main execution logic
maps = list_all_maps()

if len(maps) > 0:
    # If maps exist, list them
    for map_obj in maps:
        arcpy.AddMessage(f"Existing map: {map_obj.name}")
    result_map, action_taken = rename_or_create_map()

    if action_taken:
        summary_message = f"Map '{result_map.name}' has been renamed to {county}_County."
    else:
        summary_message = "No action taken."
else:
    # No maps exist, inform the user
    result_map, action_taken = rename_or_create_map()
    if action_taken:
        summary_message = f"Map has been renamed to {county}_County."
    else:
        summary_message = "No map created."

# Print summary message
print(summary_message)
arcpy.AddMessage(summary_message)
```

### Add GeoTIFFs
**Add GeoTIFF files to the active map:**

*How It Works:*
- *The script scans the county directory for valid GeoTIFF files, excluding those with "TMorth" in the filename.*
- *It matches the filenames with a list of quadrangle names and adds the corresponding GeoTIFF files to the active map.*

*The order of the quads in the list determines the order in which the GeoTIFFs are added to the map. The last item in the list is added first, with the topmost GeoTIFF covering the northwest corner of the county and the bottommost covering the southeast corner.*

```python
import arcpy
import os
import time  # Import time module to add a delay

def add_geotiffs_to_map(county_dir, quads, map_obj, delay_seconds=1):
    # Ensure the county directory exists
    if not os.path.isdir(county_dir):
        print(f"The directory {county_dir} does not exist.")
        return

    # Step 1: Get all GeoTIFF files within the county directory
    valid_geotiffs = []

    # Walk through the county directory to find all .tif files excluding those with "TMorth" in the filename
    for dirpath, dirnames, filenames in os.walk(county_dir):
        for filename in filenames:
            if filename.lower().endswith('.tif') and "tmorth" not in filename.lower():
                valid_geotiffs.append(os.path.join(dirpath, filename))

    # Step 2: Provide feedback on the number of GeoTIFFs found
    if len(valid_geotiffs) == 0:
        print("No valid GeoTIFFs found in the directory.")
    else:
        print(f"Found {len(valid_geotiffs)} valid GeoTIFF(s). Adding to the map...")

        added_geotiffs = set()
        for quad in quads:
            # Normalize the quad name by removing spaces and converting to lowercase
            quad_normalized = quad.replace(" ", "").lower()

            # Find matching GeoTIFFs
            matching_geotiffs = [geotiff for geotiff in valid_geotiffs 
                                 if quad_normalized in os.path.basename(geotiff).replace(" ", "").lower()]

            # Add the matching GeoTIFF to the map (if it's not already added)
            for geotiff in matching_geotiffs:
                if geotiff not in added_geotiffs:
                    try:
                        # Normalize path to always use forward slashes
                        geotiff = geotiff.replace("\\", "/")  # Convert backslashes to forward slashes
                        
                        # Handle spaces in GeoTIFF filenames by ensuring they're properly processed
                        if " " in os.path.basename(geotiff):
                            print(f"GeoTIFF with spaces detected: {geotiff}")
                        
                        map_obj.addDataFromPath(geotiff)
                        added_geotiffs.add(geotiff)  # Track as added
                        print(f"Added {geotiff} to the map.")
                        
                        # Introduce a time lag before adding the next GeoTIFF
                        time.sleep(delay_seconds)  # Delay in seconds

                    except Exception as e:
                        print(f"Error adding {geotiff} to the map: {e}")

        print(f"{len(added_geotiffs)} GeoTIFFs added to the map.")

# Assuming the map object is already available in ArcGIS Pro
# Get the current map from ArcGIS Pro
aprx = arcpy.mp.ArcGISProject("CURRENT")
map_obj = aprx.activeMap  # Get the active map in the project

# Check if the directory exists and add GeoTIFFs to the map
if os.path.isdir(county_directory):
    add_geotiffs_to_map(county_directory, quads, map_obj, delay_seconds=1)  # Set delay to 2 seconds between each addition
else:
    print(f"The county directory {county_directory} does not exist.")
```

**Group GeoTIFFs in the Contents pane:**

*How It Works:*
- *The script checks if a "topos" group already exists in the active map; if not, it creates one.*
- *It loops through all layers in the map, adding all .tif raster layers to the "topos" group in reverse order.*
- *It removes any .tif layers that are not part of the "topos" group, cleaning up the map.*

```python
import arcpy

# Get the active map from the currently open project
aprx = arcpy.mp.ArcGISProject("CURRENT")  # Refers to the active project
map_obj = aprx.activeMap  # Get the active map

# Check if the map is valid
if map_obj is None:
    print("No active map found.")
else:
    # Create a new group layer named "topos" if it doesn't already exist
    group_name = "topos"
    existing_groups = [layer for layer in map_obj.listLayers() if layer.isGroupLayer]

    # Check if the "topos" group already exists, if not, create it
    topos_group = None
    for group in existing_groups:
        if group.name == group_name:
            topos_group = group
            break

    if topos_group is None:
        # Create a new group layer called "topos"
        topos_group = map_obj.createGroupLayer(group_name)
    
    # Loop through layers and add all GeoTIFFs (.tif) to the "topos" group in reverse order
    for layer in map_obj.listLayers():  # Reverse the list of layers
        if layer.isRasterLayer and layer.name.lower().endswith(".tif"):  # Check for .tif layers
            # Move the GeoTIFF layer to the "topos" group
            map_obj.addLayerToGroup(topos_group, layer)
    
    # Remove all ungrouped .tif layers (those not in the "topos" group)
    for layer in map_obj.listLayers():
        if layer.isRasterLayer and layer.name.lower().endswith(".tif"):
            # Check if the layer is not part of the "topos" group
            if layer not in topos_group.listLayers():
                # Remove the ungrouped .tif layer from the map
                map_obj.removeLayer(layer)
    
    print(f"All .tif layers have been grouped under '{group_name}' group in reversed order, and ungrouped layers have been removed.")
```

### Add County Boundaries
**Add the *ky_counties* feature class and update symbology to create a boundary outline of the county of interest:**

*How It Works:*
- *The script checks if the specified feature class exists in the given geodatabase.*
- *It adds the feature class to the active map and customizes its symbology with a transparent fill, colored outline, and offset effect.*
- *It applies a definition query to filter the layer based on the given county name and saves the project.*

```python
import arcpy
import os

# Specify the feature class name you are looking for
fc_name = "ky_counties"

# Search for the feature class in the geodatabase
if gdb_path and os.path.isdir(gdb_path):
    # Construct the path to the feature class
    fc_path = os.path.join(gdb_path, fc_name)
    
    if arcpy.Exists(fc_path):
        print(f"Feature class '{fc_name}' found in {gdb_path}.")
        
        # Get the active project and map
        aprx = arcpy.mp.ArcGISProject("CURRENT")
        map_obj = aprx.activeMap

        # Add the feature class to the map
        new_layer = map_obj.addDataFromPath(fc_path)
        
        # Apply the symbology with a SimpleRenderer
        sym = new_layer.symbology
        sym.updateRenderer('SimpleRenderer')  # Use the SimpleRenderer
        
        # Apply the symbol settings
        symbol = sym.renderer.symbol
        
        # Set color to transparent fill
        symbol.color = {'RGB': [0, 0, 0, 0]}  # Transparent fill (alpha 0)
        
        # Set outline color (red = 169, green = 0, blue = 230)
        symbol.outlineColor = {'RGB': [169, 0, 230, 100]}  # Outline: RGB(169, 0, 230) with full opacity
        
        # Set outline width to 5 points
        symbol.outlineWidth = 5  # Outline width set to 5 points
        
        # Apply an outline offset effect (5 pts)
        symbol.outlineOffsetX = 5  # Horizontal offset
        symbol.outlineOffsetY = 5  # Vertical offset
        
        # Apply the updated symbology back to the layer
        new_layer.symbology = sym
        
        # Apply the definition query based on the "county" variable
        new_layer.definitionQuery = f"NAME = '{county}'"
        
        print(f"Definition query applied: NAME = '{county}'")
        
        # Save the project after changes
        aprx.save()
    else:
        print(f"Feature class '{fc_name}' does not exist in the specified geodatabase.")
else:
    print("Invalid geodatabase path. Please check and try again.")
```

### Create Cemetery Polygon Feature Class
**Create a copy of *Cemetery_PolygonFC_Template*, rename, and change symbology:**

*How It Works:*
- *The script copies the "Cemetery_PolygonFC_Template" feature class to a new feature class, named after the county.*
- *It checks if the new layer is already in the map; if not, it adds the new feature class to the map.*
- *It applies custom symbology to the new layer, setting it to a semi-transparent bright pink with a transparent outline, then saves the project.*

```python
import arcpy
import os

# Define the source feature class and the new name
source_fc = "Cemetery_PolygonFC_Template"
new_fc_name = f"{county}_County"

# Construct the full paths for source and destination feature classes
source_fc_path = os.path.join(gdb_path, source_fc)
new_fc_path = os.path.join(gdb_path, new_fc_name)

# Check if the source feature class exists
if arcpy.Exists(source_fc_path):
    # Copy the feature class to the new location with the new name
    arcpy.CopyFeatures_management(source_fc_path, new_fc_path)
    print(f"Polygon cemetery feature class created for {county} County: '{new_fc_name}'.")
else:
    print(f"Feature class '{source_fc}' does not exist in the specified geodatabase.")

# Get the map and check if the layer is already present before adding it
aprx = arcpy.mp.ArcGISProject("CURRENT")
map_obj = aprx.activeMap

# Check if the layer is already in the map, and if not, add it
layer_exists = False
for layer in map_obj.listLayers():
    if layer.name == new_fc_name:
        layer_exists = True
        break

if not layer_exists:
    new_layer = map_obj.addDataFromPath(new_fc_path)
    print(f"Added '{new_fc_name}' to the map.")
else:
    print(f"Layer '{new_fc_name}' is already in the map.")

# Access the symbology of the new layer
new_layer = map_obj.listLayers(new_fc_name)[0]
sym = new_layer.symbology

# Set the symbology to a SimpleRenderer
sym.updateRenderer("SimpleRenderer")

# Apply the semi-transparent bright pink color (Red 255, Green 0, Blue 197, 45% transparency)
sym.renderer.symbol.color = {'RGB': [255, 0, 197, 0.25]}  # 25% transparency

# Make the outline completely transparent
sym.renderer.symbol.outlineColor = {'RGB': [255, 255, 255, 0]}  # Transparent outline (alpha = 0)

# Apply the updated symbology to the layer
new_layer.symbology = sym

# Save the project after making changes
aprx.save()

print(f"Symbology applied: Semi-transparent bright pink fill with a transparent outline.")
```

### Add Cemetery Point Data
**Add *KYCemPts_FindaGrave_01292025*, *Archaeology_HistoricCem_Centerpoints*, and *KYCemPts_FindaGrave_01292025* feature classes to the map with custom symbology:**

*How It Works:*
- *The script checks if each feature class exists in the geodatabase and adds it to the active map.*
- *It applies specific symbology based on the feature class name, using symbols such as 3D pushpins and circle markers.*
- *After applying symbology to the layers, the script saves the ArcGIS project.*

```python
import arcpy
import os

# Define the feature classes
feature_classes = [
    "KY_CemData_Draft2013",
    "Archaeology_HistoricCem_Centerpoints",
    "KYCemPts_FindaGrave_01292025"
]

# Get the active ArcGIS project and map
aprx = arcpy.mp.ArcGISProject("CURRENT")  # Get the current ArcGIS Project
map_obj = aprx.activeMap  # Get the active map

# Function to apply symbology to point feature classes (for color and outline)
def apply_point_symbology(layer, fill_color, outline_color, outline_width, size):
    sym = layer.symbology
    sym.updateRenderer("SimpleRenderer")  # Apply the SimpleRenderer for point features
    
    # Set the fill color (using RGB)
    sym.renderer.symbol.color = {'RGB': fill_color}
    
    # Set the outline color (using RGB)
    sym.renderer.symbol.outlineColor = {'RGB': outline_color}
    
    # Set the outline width
    sym.renderer.symbol.outlineWidth = outline_width
    
    # Set the point size
    sym.renderer.symbol.size = size
    
    # Apply the changes to the layer's symbology
    layer.symbology = sym

# Function to apply symbol from gallery (for circle symbol)
def apply_circle_symbol(layer):
    sym = layer.symbology
    sym.updateRenderer("SimpleRenderer")  # Apply the SimpleRenderer for point features
    
    # Apply a circle symbol from the symbol gallery
    sym.renderer.symbol.applySymbolFromGallery("Circle 6")
    
    # Apply the changes to the layer's symbology
    layer.symbology = sym

# Function to apply 3D Billboard pushpin symbols
def apply_pushpin_symbol(layer, symbol_name):
    sym = layer.symbology
    sym.updateRenderer("SimpleRenderer")  # Apply the SimpleRenderer for point features
    
    # Apply the pushpin symbol from the gallery (Style: 3D Billboards)
    sym.renderer.symbol.applySymbolFromGallery(symbol_name)
    
    # Apply the changes to the layer's symbology
    layer.symbology = sym

# Loop through each feature class and add them to the map with appropriate symbology
for fc in feature_classes:
    fc_path = os.path.join(gdb_path, fc)  # Get the full path to the feature class
    
    if arcpy.Exists(fc_path):
        # Add the feature class to the map
        layer = map_obj.addDataFromPath(fc_path)
        
        # Apply specific symbology based on the feature class name
        if fc == "KY_CemData_Draft2013":
            # Apply "Green Pushpin 1" from 3D Billboards style
            apply_pushpin_symbol(layer, "Green Pushpin 1")
        
        elif fc == "Archaeology_HistoricCem_Centerpoints":
            # Apply "Blue Pushpin 1" from 3D Billboards style
            apply_pushpin_symbol(layer, "Blue Pushpin 1")
        
        elif fc == "KYCemPts_FindaGrave_01292025":
            # Apply Circle 6 symbol from the gallery
            apply_circle_symbol(layer)
        
        print(f"Added and symbolized {fc} to the map.")
    else:
        print(f"Feature class '{fc}' does not exist in the geodatabase.")

# Optionally, save the project after making changes
aprx.save()
```