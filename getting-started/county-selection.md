---
title: County Selection
parent: Getting Started
nav_order: 1
---

# County Selection
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Define the County-Quadrangle Dictionary:

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

## Create a "county" variable:

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