---
layout: post
title: "Underscore template in Jade template"
description: "use plain text block in a tag"
category:
tags: [jade, underscore, template, express]
---
{% include JB/setup %}

I'm trying [Jade](http://jade-lang.com/) these days. It's a tidy and clean way to write templates when using Express as Web Server Engine.

But an unexpected problem came and perplexed me, which is how to use *Underscore Template* in *Jade Template*. I need to render the Underscore template in browser, but Jade will render the template as rendering a normal Jade template.

So the problem is how to let Jade skip the Underscore template code or how to let Jade render the template to what we need. After some Google searching I found it simple to solve this problem: just use `script.` to insert a plain text block as template code. It's called *Block in a Tag*. You can find more details on this page [http://jade-lang.com/reference/plain-text/](http://jade-lang.com/reference/plain-text/).


	script(type='text/template' id='js-template-tree').
	  <ul>
	    <%  _.each(elements, function(element) { %>
	    <%    if (element.type === 'file') { %>
	            <li class="tree-file">
	              <a href="#">
	                <i class="octicon octicon-file-text"></i>
	                <span><%= element.name %></span>
	              </a>
	            </li>
	    <%    } else { %>
	            <li class="tree-dir">
	              <a href="#">
	                <i class="octicon octicon-file-directory"></i>
	                <span><%= element.name%></span>
	              </a>
	              <%= func({ elements: element.children, func: func}) %>
	            </li>
	    <%    } %>
	    <%  }); %>
	  </ul>


