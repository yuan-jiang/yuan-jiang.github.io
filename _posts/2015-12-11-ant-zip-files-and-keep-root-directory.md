---
layout: post
title: Ant zip files and keep root directory
date: 2015-12-11 11:50:00 +0800
author: Yuan Jiang
tags: automation
---

An example of using apache Ant to create a zip file and keep the root directory.

{% highlight xml %}
<target name="package" depends="compile">
    <tstamp>
        <format property="ts" pattern="yyyyMMddhhmmss"></format>
    </tstamp>

    <property name="zipfolder" value="projectname-${ts}"></property>

    <mkdir dir="${zipfolder}"></mkdir>

    <copy todir="${zipfolder}">
        <fileset dir="${basedir}" includes="folderX/**, fileX"></fileset>
    </copy>

    <zip destfile="${zipfolder}.zip">
        <zipfileset dir="${basedir}">
            <include name="${zipfolder}/**/**"></include>
        </zipfileset>
    </zip>

    <delete dir="${zipfolder}"></delete>
</target>
{% endhighlight %}
