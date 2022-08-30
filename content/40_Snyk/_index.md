---
title: "Module 1: Snyk"
chapter: true
weight: 40
---

# Snyk 

### Setting Up The Workshop: AWS Hosted Or Self-paced
By utilizing the Hugo language localization settings, directing the workshop towards a specific setup can be simplified. The `Language` setting in the `config.toml` file will allow you to distinguish between having one option or both. Commenting out one of the languages will hide all files that are related to that setup. By default, only the self-guided setup will be enabled. To enable switching, set `disableLanguageSwitchingButton` to `false` in the `config.toml`. If you want to have only the Event Engine setup, set the `defaultContentLanguage` at the top of the `config.toml` file to `ee`.

### Working With Hugo Markdown and Shortcode
The following links will supply you with all the reference documentation about Hugo markdown. For more experienced developers, inline HTML is also an option to add more customization. For example `<p style='text-align: left;'>` inline will allow you to adjust your text placement.

### Markdown and Shortcode Resources
{{% notice tip %}}
The following links are your go-to resource for markdown and shortcode reference in building your workshop: <br>
* Markdown cheat sheet https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet <br>
* Learn theme markdown https://learn.netlify.app/en/cont/markdown/ <br>
* Menu extras and shortcuts https://learn.netlify.app/en/cont/menushortcuts/ <br>
* Using Font Awesome Emoji's <i class="fas fa-heart"></i> https://learn.netlify.app/en/cont/icons/ to help your page pop <i class="fas fa-glass-cheers"></i>
{{% /notice %}}

### The "More" Menu Section
This section of the menu on the left is designed to add additional resources that are related to the workshop but not necessarily part of the workshop itself. To modify these links, edit the sections marked `[[menu.shortcuts]]` in the `config.toml` located in the root folder. The "name" portion will be what is displayed in the menu. The "url" should be the address of the link. The "weight" setting will adjust the display order, similar to the other "weight" settings utilized in indexes and modules mentioned previously.