---
title: Books
description: "All the books I've bothered to read and write a review for."
author: ""
show_post_thumbnail: true
show_author_byline: true
show_post_date: false
# for listing page layout
layout: list-grid # list, list-sidebar, list-grid

# for list-sidebar layout
sidebar: 
  title: A Sidebar for Your Projects
  description: |
    Projects can be anything!
    Check out the _index.md file in the /project folder 
    to edit this content.
  author: "The R Markdown Team @RStudio"
  text_link_label: ""
  text_link_url: ""
  show_sidebar_adunit: false # show ad container

# set up common front matter for all individual pages inside project/
cascade:    
  show_author_byline: true
  show_post_date: true
  show_comments: false # see site config to choose Disqus or Utterances
  # for single-sidebar layout only
  sidebar:
    text_link_label: View all projects
    text_link_url: /project/
    show_sidebar_adunit: true # show ad container
---