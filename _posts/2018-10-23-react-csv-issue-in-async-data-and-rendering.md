---
layout: post
title: React-csv issue in async data and rendering
date: 2018-10-23 12:55:00 +0800
tags: js javascript
---

In frontend development sometimes you need to export the data (usually in JSON format) as downloadable csv format, and for those working on ReactJS related web applications, there is a package named [react-csv](https://github.com/abdennour/react-csv) that can save you from breaking the DRY... However, you may also encounter issues when applying this package in some specific scenarios, such as the one I'm writing down soon which involves in data being loaded asyncronously but needed for initial UI rendering.

## Issue
To use the react-csv package for transferring JSON data into CSV, you can use either the CSVLink or CSVDownload component, like this:
{% highlight javascript %}
import { CSVLink } from "react-csv";

<CSVLink data={data}, headers={csvHeaders}, filename={"export.csv"}>Export as CSV</CSVLink>
{% endhighlight %}
But a potential problem is that the {data} used for rendering the CSVLink component must be available during which the component is rendered, and if the data is being pulled by async call from other sources, chances are that the {data} hasn't been ready yet when the CSVLink is being rendered, so it ends up rendering a component with empty data, which further causes the downloaded csv file to be empty.

## Solution
- Do not render the CSVLink component during page initial rendering because of async data loading. Instead, set a flag to determine whether the CSVLink should be rendered or not based on data availability, and render a normal button which when clicked triggers the CSVLink component to be rendered, and then simulate clicking to the CSVLink componnet to proceed with CSV downloading, like below:
{% highlight javascript %}
<Button type="primary" icon="download" onClick={this.downloadHandler}><FormattedMessage id="btn.download" /></Button>
{
    this.state.active ? 
    <CSVLink data={data} headers={csvHeaders} filename={"export.csv"} ref={this.exportBtn}></CSVLink> :
    null
}
{% endhighlight %}
- Handle the click event in the normal button and further proceed with CSV download in CSVLink:
{% highlight javascript %}
  downloadHandler = () => {
    if (data) {
      this.setState({
        active: true
      });
      if (this.isCsvFileReady()) {
        this.exportBtn.current.link.click();
      } else {
        setTimeout(() => {
          if (this.isCsvFileReady()) {
            this.exportBtn.current.link.click();
          }
        }, 3000);
      }
    }
  }

  isCsvFileReady = () => {
    return this.exportBtn && 
    this.exportBtn.current &&
    this.exportBtn.current.link &&
    this.exportBtn.current.link.click &&
    typeof this.exportBtn.current.link.click === 'function';
  }
{% endhighlight %}

## Summary
The original issue of empty csv export is solved successfully with the above solution, but it is not perfect and there is still some issue, for example, when the "active" flag is set to "true" by the setState method to render the CSVLink: because 1) the setState method is also async; 2) the CSVLink rendering also takes time, it may not become ready before the click handling happens, for which a quick solution so far is to set a timeout of 3 seconds for the component to render and CSV data to generate, and it only impacts the very first click and seems to work fine.