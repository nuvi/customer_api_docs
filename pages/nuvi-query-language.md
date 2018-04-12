# NUVI Query Language (NQL)

**NUVI Query Language** (NQL**) is an original, domain specific language (DSL) created at NUVI.

We designed it to be a concise and efficient way to create and read complex rules used to filter and sort text-based data, specifically in regards to social media posts.

The structure was designed for large and small **rules sets** which typically have long lists of **values** and conditionals used to match specific Natural Language based data, like social media posts.

## Structure

A rule consists of the following arguments:

```
[field] [operator] [value(s)] [connector*]
```
*Connector is optional and not used in simple rules

### Example 1 - Simple Rule 
Find all mentions that include the word `football` or `baseball`.

```
word ANY football, baseball
```

### Example 2 - Simple Rule with Connector
Find all mentions that include the word `football` OR `baseball` but IGNORE mentions that include the work `basketball`

```
word ANY football, baseball AND word NOT ANY basketball
```

### Example 3 - Complext Rule with Multiple Connectors

Find metions that **include the word `holiday`** but **ignores birthday mentions including words `democratic` `republican`**. We also want to make sure we are only tracking mention written in `english` and within a specified geographic region. The rule definition would look like this:

```
word ANY happy birthday AND
word NOT_ANY birthday, democratic, republican AND
language ANY en, sp AND 
coordinate ANY (-105.27346517 40.01924738 16093.4), 
(-155.27346517 45.01924738 8046.7)
```
    
Rules are designed to be chained together and are seperated by the presence of a `connector`. Arguments are parsed by use of their corresponding reserved words.

### Arguments

| Argument | Restrictions | Values|
| --- | --- | --- |
| field | **Must be lowercase** | `word`, `character`, `domain`, `language`, `network`, `place`, `proximity`, `profile`, `coordinate`, `location`, `source` |
| operator| **Must be capitalized** | `ANY`, `ALL`, `NOT ANY`, `NOT ALL` |
| value(s) | --- | Come in a variety of forms, depending on the **Field** usage. See table below |
| connectors| **Must be capitalized** This is optional and should be used to chain clauses. | `AND` , `OR` |

### Field Argument Options
------------------------

| Field Value | Description | Example |
| --- | --- | --- |
| word | comma separated words | word ANY basketball, NBA, All Star Weekend |
| character | Subsets of a word. As Football, Baseball, and Basketball all contain `ball` this character would pick up all three words. | character ANY ball, fall |
| domain | comma separated domains | domain ANY www.domain.com, sub.domain.com, www.domain2.com/path |
| language | comma separated language ids. See list below for id mapping to languages. | language ANY en, sp, fr |
| network | comma separated network name. See list below for network names. Note, same networks require premium access. | network ANY twitter, facebook, instagram |
| proximity | check for word within (n) words from specified word(s). | proximity ANY 4 baseball, champions |
| profile | Mentions created by profile name. The '@' symbols should NOT be included. For example, 'cnn' is valid, but'@cnn'is invalid. comma separated. | profile ANY twitter, cnnbrk  |
| coordinate | Mention within geographic range. (latitude, longitude, radius in meters) | coordinate ANY (108.123, 48.000, 10000), (148.123, 28.000, 20000), (58.123, 28.000, 1000) |
| location | Specify region based off of country/state/region abbreviations | location ANY USA(UT, CO, WA), UK(LN, WL, BR), AU(QL, PE) |
| source | Mentions for specific users on a specifc social network. network_name(user_id1, user_id2) | source instagram(123456, 987654, 10101212) |

