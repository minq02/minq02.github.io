---
title: ""
layout: home
permalink: /
classes: wide
redirect_from:
  - /portfolio/
  - /portfolio
---

## About Me

Hi, I am Min Kyu :wave:

## Experience
<table>
  <tbody>
    <tr>
      <td style = "border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/ar.jpg" alt="ar" width="60"></td>
      <td style = "border-bottom-width:0;">
        <strong>Amazon Robotics</strong> <br> 01/2025 - Present <br> Robotics System Eng Co-op</td>
      <td style = "border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/hinetics.jpg" alt="hinetics" width="60"></td>
      <td style = "border-bottom-width:0;">
        <strong>Hinetics</strong> <br> 08/2023 - 12/2023 <br> Mechanical Eng Intern</td>
    </tr>
  </tbody>
</table>

## Education
<table>
  <tbody>
    <tr>
      <td style="border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/umich.jpg" alt="umich" width="60"></td>
      <td style="border-bottom-width:0;">
        <strong>University of Michigan</strong> <br> 08/2024 - Present <br> M.S. in Robotics
      </td>
      <td style="border-bottom-width:0;"><img src="{{site.baseurl}}/assets/images/main/uiuc.jpg" alt="umich" width="60"></td>
      <td style="border-bottom-width:0;">
        <strong>University of Illinois Urbana-Champaign</strong> <br> 08/2020 - 05/2024 <br> B.S. in Mechanical Engineering
      </td>
    </tr>
  </tbody>
</table>

## Projects

<div class="container">
  <div class="image-container">
    <a href="/portfolio_page/omnids">
      <img src="{{site.baseurl}}/assets/images/omnids.gif" alt="omnids">
    </a>
  </div>
  <div class="text-container">
    <div class="header-row">
      <a href="/portfolio_page/omnids" class="title-link">
        <h3>Action Chunking with Transformers (ACT) for Assistive Action Prediction</h3>
      </a>
    </div>
    <div class="text-content">
      <p>This project aims to enhance human-robot collaboration with Omnid Mocobots by implementing imitation learning approaches (Action Chunking with Transformers and Diffusion Policy) to predict human intent during co-manipulation tasks.</p>
    </div>
  </div>
</div>

<style>
.container {
  display: flex;
  margin-bottom: 10px;
  gap: 10px;
}

.image-container {
  flex: 0 0 200px;
  height: 100px;
  overflow: hidden;
}

.image-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
  transition: opacity 0.2s;
}

/* Special handling for logo-style images in the experience/education tables */
table img {
  width: 60px;
  height: 60px;
  object-fit: contain;
}

.image-container img:hover {
  opacity: 0.8;
}

.text-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100px;
  justify-content: flex-start;
}

.header-row {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 0.25rem;
}

.header-row h3 {
  margin: 0;
  font-size: 0.8rem;
  color: #333;
  transition: color 0.2s;
  line-height: 1.2;
}

.title-link {
  text-decoration: none;
  color: inherit;
}

.title-link:hover h3 {
  color: #0066cc;
  text-decoration: underline;
}

.text-content p {
  margin: 0;
  font-size: 0.6rem;
  line-height: 1.4;
  color: #666;
}
</style>