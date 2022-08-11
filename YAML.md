# YAML
[Документация](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)
[Online converter YAML to JSON](https://jsonformatter.org/yaml-to-json)


```
name: Martin D'vloper
job: Developer
    foo
skill: Elite
employed: True
foods:
  - Apple
  - Orange
  - Strawberry
  - Mango
languages:
  perl: Elite
  python: Elite
  pascal: Lame
education: | 
  4 GCSEs
  3 A-Levels BSc in the Internet of Things
  BSc in the Internet of Things
lorem: > 
  4 GCSEs
  3 A-Levels BSc in the Internet of Things
  BSc in the Internet of Things
  ```
  
  Ковертируется в:
  ```
  {
    "name": "Martin D'vloper",
    "job": "Developer foo",
    "skill": "Elite",
    "employed": true,
    "foods": [
        "Apple",
        "Orange",
        "Strawberry",
        "Mango"
    ],
    "languages": {
        "perl": "Elite",
        "python": "Elite",
        "pascal": "Lame"
    },
    "education": "4 GCSEs\n3 A-Levels BSc in the Internet of Things\nBSc in the Internet of Things\n",
    "lorem": "4 GCSEs 3 A-Levels BSc in the Internet of Things BSc in the Internet of Things\n"
}
```

#yaml