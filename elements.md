---
layout: none
title: playground
description: a little bit of game...
nav-menu: true
---

<head>
    <meta charset="UTF-8">
    <meta name = "viewport" content = "width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1">
    <title>2048 game</title>
    <link rel="stylesheet" type="text/css" href="assets/css/2048.css">
</head>
<body>
    <div class="game-container" style="overflow: scroll;">
        <div class="container-header">
            <div class="banner">
                <div class="scoreboard">
                    <span class="score-title">Score</span>
                    <span class="score-number">0</span>
                </div>
                <div class="title">
                    <span>2048</span>
                </div>
            </div>
        </div>
        <div class="game-board">
            <table cellspacing="10" class="game-table">
                <tr>
                    <td class="cell a 1"></td>
                    <td class="cell a 2"></td>
                    <td class="cell a 3"></td>
                    <td class="cell a 4"></td>
                </tr>
                <tr>
                    <td class="cell b 1"></td>
                    <td class="cell b 2"></td>
                    <td class="cell b 3"></td>
                    <td class="cell b 4"></td>
                </tr>
                <tr>
                    <td class="cell c 1"></td>
                    <td class="cell c 2"></td>
                    <td class="cell c 3"></td>
                    <td class="cell c 4"></td>
                </tr>
                <tr>
                    <td class="cell d 1"></td>
                    <td class="cell d 2"></td>
                    <td class="cell d 3"></td>
                    <td class="cell d 4"></td>
                </tr>
            </table>
        </div>
    </div>
    <script type="text/javascript" src="assets/js/2048.js"></script>
</body