# Cultures

## Default cultures

* aserai
* battania
* empire
* knuzait
* sturgia
* vlandia



## Check

    if (hero.Culture.StringId == "sturgia") ..


## Notes

Village's culture doesn't affect settlement loyalty. Only castle and town culture does.

If you want a single village to have its own troops then you can change that village to a new culture. It won't affect the castle/town it's bound to.


## XML

### spcultures.xml


### module_strings.xml

Related strings:

``` xml
  <string id="str_culture_rich_name.baltic" text="baltic" />
  <string id="str_faction_official.baltic" text="King" />
  <string id="str_faction_ruler.baltic" text="King" />
  <string id="str_neutral_term_for_culture.baltic" text="balts" />
  <string id="str_culture_description.baltic" text="The Balts, ..." />
  <string id="str_faction_official.baltic_f" text="queen" />
```

### Town menu picture

New culture requires new town menu sprite, without it it's empty:

![](https://imgur.com/NsKHhOg.png)