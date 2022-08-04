<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.0/css/bootstrap.min.css">
  <link rel = "stylesheet" type = "text/css" href = "http://underd0g.co/projects/projectstyle.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.0/js/bootstrap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"></script>


</head>
<body>
  <style>

  @font-face {
    font-family: myfont;
    src: url(slkscr.ttf);
    font-size: 14px;
  }

body {
  font-family: myfont;
  font-weight: 300;
  font-size: 1.2em;
  line-height: 3.7em;
  background: #282a36;
}

p{
  background-color: #222;
  color: white;

}

#top{
  background: #222;
}

#col{
  color: white;
  
}
text{
  stroke-width: 0.09;
  stroke: white;
  fill: white;
}
.node {
  color: white;
  stroke-width: 1.0px;
  fill: white;
}


.link {
  stroke: #555;
  stroke-width: 1.0px;
}

.node text {
  font: 17px "SauceCodePro Nerd Font", monospace ;

}


input {
  width: 35%;
  background: transparent;
  border: none;
  font-family: myfont;
  font-size: 36;
  color: #c0a79a;
  padding-top: 50px;
}

input:focus {
  outline: 0;
}

input::placeholder {
  color: #fbfbfb;
}
form-search {
  margin: 1em 2em;
}

</style>


<div class="container"><center>
    <form id="form-search" action="https://www.startpage.com/sp/search?query=" method="get" autocomplete="off">
                    <input id="search" placeholder="" type="text" name="q" autofocus="">
                </form>
              </center>
             <div class="row" align='center'>
      <div class="col-xsm-12" id = "col">
  <script src="https://d3js.org/d3.v3.min.js"></script>
  <script>

    //setup for the d3 SVG


// testing a resize method using the browser measurements 

//  var width = window.innerWidth, 
//      height = window.innerHeight;

  var width = 1300, 
      height = 685;

    //Set up the colour scale
    var color = d3.scale.category20c()
  //   var color = d3.scaleOrdinal() // D3 Version 4
  // .domain(["9", "7", "1"])
  .range(["#50fa7b", "#ff79c6" , "#f1fa8c", "#ff5555", "#bd93f9", "#ffb86c", "#8be9fd", "#6272a4", "#f8f8f2", "#44475a", "#44475a", "#282a36"]);
    //Set up the force layout
    var force = d3.layout.force()
      .gravity(0.05)
      .charge(-200)
      .linkDistance(70)
      .size([width, height]);


    //yes this is an svg
    var svg = d3.select(".col-xsm-12").append("svg")
      .attr("width", width)
      .attr("height", height);







    // hard-code some json
    var graph = {
      "nodes":[
      {"name":"  ","group":2, url: "http://youtube.com"},
      {"name":"tidy tuesday","group":2,url:"https://www.youtube.com/c/AndrewCouch/videos"},
      {"name":"gotham chess","group":2,url:"https://www.youtube.com/c/GothamChess"},
      {"name":"   ","group":3, url: "https://reddit.com"},
      {"name":"   ","group":9},
      {"name":"rbloggers","group":9, url: "https://www.r-bloggers.com/"},
      {"name":"chess digits","group":9,url:"https://web.chessdigits.com/home"},
      {"name":"hackernews","group":9, url: "https://news.ycombinator.com/"},
      {"name":"r/rlanguage","group":3, url: "https://www.reddit.com/r/Rlanguage/"},
      {"name":" r/chess ","group":3, url: "https://www.reddit.com/r/chess/"},
      {"name":" r/python ","group":3, url: "https://www.reddit.com/r/Python/"},
      {"name":" r/math ","group":3, url: "https://www.reddit.com/r/math/"},
      {"name":" rstudio-blog ","group":9, url: "https://www.rstudio.com/blog/"}

      ],
      "links":[
      {"source":0,"target":0,"value":0},
      {"source":1,"target":0,"value":1},
      {"source":2,"target":0,"value":1},
      {"source":3,"target":3,"value":0},
      {"source":4,"target":4,"value":0},
      {"source":5,"target":4,"value":1},
      {"source":6,"target":4,"value":1},
      {"source":7,"target":4,"value":1},
      {"source":8,"target":3,"value":1},
      {"source":9,"target":3,"value":1},
      {"source":10,"target":3,"value":1},
      {"source":11,"target":3,"value":1},
      {"source":12,"target":4,"value":1}




    ]
    }

  //Creates the graph data structure out of the json data
      force.nodes(graph.nodes)
        .links(graph.links)
        .start();

  //Create all the line svgs but without locations yet
      var link = svg.selectAll(".link")
        .data(graph.links)
        .enter().append("line")
        .attr("class", "link")
        .attr('font-color', 'white')
        .style("stroke-width", function (d) {
          return Math.sqrt(d.value);
        });

  //Do the same with the circles for the nodes
      var node = svg.selectAll(".node")
        .data(graph.nodes)
        .enter().append("g")
        .attr("class", "node")
        .call(force.drag);

      node.append("circle")
        .attr("r", 10)
        .attr('font-color', 'white')
        .style("fill", function (d) {
          return color(d.group);

        })


  //give my children  some attributes!
      node.each(function(d){
        var thisNode = d3.select(this);

        if (!d.children) {
          thisNode.append("svg:a")
            .attr("xlink:href",function(d) { return d.url; })
            .attr("target", "_blank")
            //.text(function(d) { return d.url; }) (this isnt clugy enough)
            .append("text", "fill", "white")
            .attr("dx", 8)
            .attr("dy", 3)
            .attr("text-anchor", "start")
            .attr('font-color', 'white')
            .attr('font-color', 'white')

            .text( function(d) { return d.name; });
        } else {
          thisNode.append("text")
            .attr("dx", -8)
            .attr("dy", 3)
            .attr("text-anchor", "end")
            .attr('font-color', 'white')
            .text(function(d) { return d.name; });
        }
      });


  // force be with you
      force.on("tick", function () {
        link.attr("x1", function (d) {
          return d.source.x;
        })
          .attr("y1", function (d) {
            return d.source.y;
          })
          .attr("x2", function (d) {
            return d.target.x;
          })
          .attr("y2", function (d) {
            return d.target.y;
          });

        //Changed

        d3.selectAll("circle").attr("cx", function (d) {
          return d.x;
        })
          .attr("cy", function (d) {
            return d.y;
          });

        d3.selectAll("text").attr("x", function (d) {

        return d.x;
        })
          .attr("y", function (d) {
            return d.y;
          });
      });


  </script>


  </div>
  </div>
  </div>

  </div>

</body>






<details close>

 <summary>I run almost every programm over the commandline. Click the dropdown to see what I use.
</summary>


<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Application</th>
    <th class="tg-0pky">Name</th>
    <th class="tg-0pky">Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">Shell</td>
    <td class="tg-0pky"><a href="https://www.zsh.org/">zsh</a></td>
    <td class="tg-0pky">Zsh is a shell designed for interactive use, although it is also a powerful scripting language.</td>
  </tr>
  <tr>
    <td class="tg-0pky">Terminal</td>
    <td class="tg-0pky"><a href="https://alacritty.org/">alacritty</a></td>
    <td class="tg-0pky">Alacritty - A fast, cross-platform, OpenGL terminal emulator</td>
  </tr>
  <tr>
    <td class="tg-0pky">Browser</td>
    <td class="tg-0pky"><a href="https://www.qutebrowser.org/">qutebrowser</a></td>
    <td class="tg-0pky">qutebrowser is a keyboard-focused browser with a minimal GUI. It’s based on Python and Qt and free software, licensed under the GPL.

It was inspired by other browsers/addons like dwb and Vimperator/Pentadactyl.</td>
  </tr>
    <tr>
    <td class="tg-0pky">WindowManager</td>
    <td class="tg-0pky"><a href="https://i3wm.org/">i3</a></td>
    <td class="tg-0pky">i3 is a tiling window manager, completely written from scratch.</td>
  </tr>
    <tr>
    <td class="tg-0pky">Terminal Multiplexer</td>
    <td class="tg-0pky"><a href="https://github.com/tmux">tmux</a></td>
    <td class="tg-0pky">tmux is a terminal multiplexer. It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal.</td>
  </tr>
    <tr>
    <td class="tg-0pky">Media Player</td>
    <td class="tg-0pky"> <a href="https://mpv.io/">mpv</a></td>
    <td class="tg-0pky">a free, open source, and cross-platform media player</td>
  </tr>
    <tr>
    <td class="tg-0pky">Document Reader</td>
    <td class="tg-0pky"> <a href="https://pwmt.org/projects/zathura/">zathura</a></td>
    <td class="tg-0pky">zathura is a highly customizable and functional document viewer. It provides a minimalistic and space saving interface as well as an easy usage that mainly focuses on keyboard interaction.</td>
  </tr>
    <tr>
    <td class="tg-0pky">Office</td>
    <td class="tg-0pky"> <a href="https://www.r-project.org/">R + Rmd</a></td>
    <td class="tg-0pky">R is a free software environment for statistical computing and graphics</td>
  </tr>
    <tr>
    <td class="tg-0pky">Spotify</td>
    <td class="tg-0pky"> <a href="https://github.com/Rigellute/spotify-tui">Spotify-tui</a></td>
    <td class="tg-0pky">A Spotify client for the terminal written in Rust.
   <tr>
    <td class="tg-0pky">Password Manager</td>
    <td class="tg-0pky"><a href="https://www.passwordstore.org/">pass</a></td>
    <td class="tg-0pky">the standard unix password manager</td>
  </tr>
  <tr>
    <td class="tg-0pky">File Manager</td>
    <td class="tg-0pky"><a href="https://github.com/ranger/ranger">ranger</a></td>
    <td class="tg-0pky">ranger is a console file manager with VI key bindings. It provides a minimalistic and nice curses interface with a view on the directory hierarchy.</td>
  </tr>
    <tr>
    <td class="tg-0pky">Text Editor / IDE</td>
    <td class="tg-0pky"><a href="https://neovim.io/">neovim</a></td>
    <td class="tg-0pky">Vim is a highly configurable text editor built to make creating and changing any kind of text very efficient</td>
  </tr>
  <tr>
    <td class="tg-0pky">E-Mail Client</td>
    <td class="tg-0pky"><a href="https://neomutt.org/">neomutt</a></td>
    <td class="tg-0pky">"All mail clients suck. This one just sucks less." -me, circa 1995</td>
  </tr>
  <tr>
    <td class="tg-0pky">Address-BookClient</td>
    <td class="tg-0pky"><a href="https://khard.readthedocs.io/en/latest/">khard</a></td>
    <td class="tg-0pky">Khard is an address book for the Unix command line. It can read, create, modify and delete vCard address book entries. </td>
  </tr>
  <tr>
    <td class="tg-0pky">Calendar Client</td>
    <td class="tg-0pky"><a href="https://calcurse.org/">calcurse</a></td>
    <td class="tg-0pky">calcurse is a calendar and scheduling application for the command line. </td>
  </tr>
  <tr>
    <td class="tg-0pky">Theme</td>
    <td class="tg-0pky"><a href="https://draculatheme.com/">dracula</a></td>
    <td class="tg-0pky">A dark theme</td>
 </tr>
</td>
  </tr>
</tbody>
</table>

</details>

