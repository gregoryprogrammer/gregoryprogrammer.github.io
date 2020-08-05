---
layout: page
title: Games
mainmenu: false
permalink: /projects/
---

<div class="games">
{% for post in site.tags.game %}

		<div class="game">

			{% capture column %}{{ forloop.index0 | modulo:2 }}{% endcapture %}
			
			{% if column == '0' or forloop.first %}
			<!-- <div class="game-col-1"> -->
			<div class="game-left">
			{% endif %}
			
			{% if column == '1' or forloop.last %}
			<div class="game-right">
			<!-- <div class="game-col-2"> -->
			{% endif %}
			
			<a href="{{ post.url }}"> <img class="game-image" src="{{ site.imagesurl | prepend: site.baseurl}}{{ post.image }}" /> </a>

				<div class="game-description">
					<span class="post-link"> <a href="{{ post.url }}">{{ post.title }}</a> </span>
					<p>{{ post.excerpt }}</p>

					<div class="post-below">
						<a href="{{ post.playlink }}" class="playlink">PLAY GAME</a>
						<a href="{{ post.url }}" class="read-more">Read more</a>
					</div>
				</div>

			</div>

		</div> <!-- game -->

		<div class="game-between">
		{% if forloop.last == false %}
		<div class="line"></div>
		{% endif %}
		</div>


{% endfor %}
</div>






