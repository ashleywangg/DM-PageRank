# PageRank Project
In this project, I created a simple search engine for the website https://www.lawfareblog.com . This website provides legal analysis on US national security issues.

### Part 1: The Power Method

First, to check that the implementation is working, I ran the small.csv.gz graph. This was the output:
```
$ python3 pagerank.py --data=./small.csv.gz --verbose
DEBUG:root:computing indices
DEBUG:root:computing values
DEBUG:root:i = 1 accuracy = 0.25447767712596
DEBUG:root:i = 2 accuracy = 0.12880841553211212
DEBUG:root:i = 3 accuracy = 0.09160212958438495
DEBUG:root:i = 4 accuracy = 0.04564603678560257
DEBUG:root:i = 5 accuracy = 0.02756230593587382
DEBUG:root:i = 6 accuracy = 0.01249576940385664
DEBUG:root:i = 7 accuracy = 0.008513468496037285
DEBUG:root:i = 8 accuracy = 0.004558204974349984
DEBUG:root:i = 9 accuracy = 0.0027584591109352112
DEBUG:root:i = 10 accuracy = 0.0007197954691946507
DEBUG:root:i = 11 accuracy = 0.00055090226447209716
DEBUG:root:i = 12 accuracy = 0.00024219555305317044
DEBUG:root:i = 13 accuracy = 7.901999586261809e-05
DEBUG:root:i = 14 accuracy = 3.4958921892523766e-05
DEBUG:root:i = 15 accuracy = 2.5968334901342595e-05
DEBUG:root:i = 16 accuracy = 8.30589638403371647e-06
DEBUG:root:i = 17 accuracy = 2.928495039973649336e-06
DEBUG:root:i = 18 accuracy = 6.34959342660129548e-07
INFO:root:rank=0 pagerank=7.1150e-01 url=4
INFO:root:rank=1 pagerank=4.3858e-01 url=6
INFO:root:rank=2 pagerank=3.0632e-01 url=5
INFO:root:rank=3 pagerank=2.1233e-01 url=2
INFO:root:rank=4 pagerank=1.0243e-01 url=3
INFO:root:rank=5 pagerank=9.2101e-02 url=1
```

The power method was used to implement pagerank to return urls that match the sorted query string. Using the search query, the program takes in a string as the parameter and returns all urls that match the query string sorted according to their pagerank.
```
python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='corona'
INFO:root:rank=0 pagerank=3.5861e-03 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=1 pagerank=3.0460e-03 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=2.6116e-03 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=2.5390e-03 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=2.3557e-03 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=5 pagerank=2.2895e-03 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=6 pagerank=2.2727e-03 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=7 pagerank=2.2520e-03 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
INFO:root:rank=8 pagerank=2.1878e-03 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=9 pagerank=2.0339e-03 url=www.lawfareblog.com/cyberlaw-podcast-how-israel-fighting-coronavirus

```

When there is no filter limitation on the query, many of the top pages outputted are not actual articles. To narrow, our search, we can add a filter ratio. The `--filter_ratio` argument removes all pages that have more links than the specified fraction. 
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
INFO:root:rank=0 pagerank=3.4321e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.7850e-01 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.6441e-01 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.9112e-01 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=1.8001e-01 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=5 pagerank=1.728e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=6 pagerank=1.5428e-01 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=7 pagerank=1.4989e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
INFO:root:rank=8 pagerank=1.4803e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=9 pagerank=1.4469e-01 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull
```

### Part 2: The Personalization Vector
Through the implementation of a personalization vector, there is an different method of filtering which phrases to focus on. This is done through the `--personalization_vector_query` option.
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona'
INFO:root:rank=0 pagerank=6.4218e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=6.3229e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.9944e-01 url=www.lawfareblog.com/brexit-not-immune-coronavirus
INFO:root:rank=3 pagerank=1.6309e-01 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=4 pagerank=1.4409e-01 url=www.lawfareblog.com/rational-security-my-corona-edition
INFO:root:rank=5 pagerank=9.3360e-02 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=6 pagerank=8.2220e-02 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=7 pagerank=8.1420e-02 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=8 pagerank=7.9770e-02 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=9 pagerank=7.3488e-02 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
```

The `--personalization_vector_query` option can also help us find pages that are related to a subject, but do not actually mention it. This can be used to find related topics for more information.
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona' --search_query='-corona'
INFO:root:rank=0 pagerank=6.4218e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=6.3229e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.5947e-01 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=9.3360e-02 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=4 pagerank=7.0277e-02 url=www.lawfareblog.com/fault-lines-foreign-policy-quarantined
INFO:root:rank=5 pagerank=6.9713e-02 url=www.lawfareblog.com/lawfare-podcast-mom-and-dad-talk-clinical-trials-pandemic
INFO:root:rank=6 pagerank=6.4944e-02 url=www.lawfareblog.com/limits-world-health-organization
INFO:root:rank=7 pagerank=5.9492e-02 url=www.lawfareblog.com/chinatalk-dispatches-shanghai-beijing-and-hong-kong
INFO:root:rank=8 pagerank=5.1245e-02 url=www.lawfareblog.com/us-moves-dismiss-case-against-company-linked-ira-troll-farm
INFO:root:rank=9 pagerank=5.1245e-02 url=www.lawfareblog.com/livestream-house-armed-services-holds-hearing-national-security-challenges-north-and-south-america
```

To test out this pagerank option with a national cyber security issue, I found related pages to run the code with Snowden.
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='snowden' --search_query='-snowden'
INFO:root:rank=0 pagerank=4.0829e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.3729e-01 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=2 pagerank=2.3729e-01 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=3 pagerank=2.2999e-01 url=www.lawfareblog.com/senate-examines-threats-homeland
INFO:root:rank=4 pagerank=2.0553e-01 url=www.lawfareblog.com/huawei-foreign-power-or-agent-foreign-power-under-fisa-insights-sanctions-case
INFO:root:rank=5 pagerank=1.9332e-01 url=www.lawfareblog.com/open-letter-gchq-threats-posed-ghost-proposal
INFO:root:rank=6 pagerank=1.8729e-01 url=www.lawfareblog.com/proposed-response-commercial-surveillance-emergency
INFO:root:rank=7 pagerank=1.7957e-01 url=www.lawfareblog.com/what-make-first-day-impeachment-hearings
INFO:root:rank=8 pagerank=1.7957e-01 url=www.lawfareblog.com/livestream-house-armed-services-committee-hearing-f-35-program
INFO:root:rank=9 pagerank=1.7944e-01 url=www.lawfareblog.com/whats-house-resolution-impeachment
```
