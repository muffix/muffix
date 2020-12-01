Hi. 👋 

I'm a Software Engineer with [Datadog](https://www.datadoghq.com), based in Munich, Germany. 🥨 Ex-[Skyscanner](https://www.skyscanner.net).

## Find me on these networks

<p align="left">
  <a href="https://stackoverflow.com/users/459812/bj%c3%b6rn-marschollek">
    <img
      src="https://raw.githubusercontent.com/MikeCodesDotNET/ColoredBadges/master/svg/social/stackoverflow.svg"
      alt="Stack Overflow"
      style="vertical-align: top; margin: 4px;"
    />
  </a>
  <a href="https://www.linkedin.com/in/bjoernmarschollek/">
    <img
      src="https://raw.githubusercontent.com/MikeCodesDotNET/ColoredBadges/master/svg/social/linkedin.svg"
      alt="LinkedIn"
      style="vertical-align: top; margin: 4px;"
    />
  </a>
  <a href="https://twitter.com/muffix">
    <img
      src="https://raw.githubusercontent.com/MikeCodesDotNET/ColoredBadges/master/svg/social/twitter.svg"
      alt="Twitter"
      style="vertical-align: top; margin: 4px;"
    />
  </a>
  <a href="https://www.meetup.com/members/194137934/">
    <img
      src="https://raw.githubusercontent.com/MikeCodesDotNET/ColoredBadges/master/svg/social/meetup.svg"
      alt="Meetup"
      style="vertical-align: top; margin: 4px;"
    />
  </a>
</p>

## Blog posts

I occasionally write engineering blog posts. Sometimes my colleagues blog about our work, too.

### Roll up to speed up: Improving OpenTSDB query performance

How to improve the query performance for Skyscanner's OpenTSDB cluster and enabling queries that previously were
impossible to serve by reducing the resolution of historic data.

👉 [Roll up to speed up: Improving OpenTSDB query performance]

### The problem that wasn’t there — and the Bosun alerts that were

_By [Annette Wilson](https://github.com/annettejanewilson)_

Annette blogged about phantom alerts that our alerting solution [Bosun] would fire every so often, paging on-call
engineers, but turn out to be false every time. The alert condition which was met and triggered the alert, would
recover on the next evaluation, only split seconds later. Subsequent investigation and resubmitting the exact same
query wouldn't show any sign of a problem, let alone the alert condition being met.

It had been annoying us for two years, but it also happened infrequently enough that investigation any efforts were
regularly abandoned without meaningful results until years later. It was mysterious and interesting enough to still
blog about it, though. Also, we really wanted to sleep comfortably again without being woken up by a false alert
looming.
The blog post describes the problem and in an addendum how I finally found the root cause.

<details>
  <summary>TL;DR - Expand here to show the root cause if you don't like exciting stories</summary>

  Our initial suspicion of a bug in Bosun turned out incorrect. When our timeseries database [OpenTSDB] serves a query,
  it uses 8 scanners to return all the required data from HBase asynchronously and proceeds to merge them before
  returning the result to the client.

  The scanners write the results to a map. The datastructure used to generate the key for tese results, however, wasn't
  thread-safe and in a rare race condition could return the same key for two scanners which meant that one overwrote
  the other's results. Bosun had incomplete data and the alert went into an unknown state, paging the on-call engineer.

  The unspectacular fix can be seen in [OpenTSDB/opentsdb#1754](https://github.com/OpenTSDB/opentsdb/pull/1754).
</details>

👉 [The problem that wasn’t there — and the Bosun alerts that were]

[Bosun]: https://github.com/bosun-monitor/bosun
[OpenTSDB]: https://github.com/OpenTSDB
[Roll up to speed up: Improving OpenTSDB query performance]: https://medium.com/@SkyscannerEng/roll-up-to-speed-up-improving-opentsdb-query-performance-83a647cba4ac
[The problem that wasn’t there — and the Bosun alerts that were]: https://medium.com/@SkyscannerEng/bosun-mystery-alerts-1167f3f1e0d3#0b99
