# chaos-hacks

regex "name"(.*?)\n(.*?)\n(.*?)"bounty":(true|\s*true) pulls out all program names with bounty:true

extracting name": "(.*?)" string

then replacing "name": " and " with noting, and we have all company names with bounties https://github.com/w9w/chaos-hacks/blob/main/company_names_with_bounty.txt

Now it's time to download all the subdomains!

Company name Acorns Grow, Inc. has such a URL https://chaos-data.projectdiscovery.io/acorns_grow,_inc..zip for subdomains' downloading, - it means all chars are lower case and whitespace replaces with _ char.

Quick python script

file = open("test.txt").readlines()
for l in file:
    l = l.lower().replace(" ", "_").replace("\n", "")
    import re; l = re.sub("^", "https://chaos-data.projectdiscovery.io/", l); l = re.sub("$", ".zip", l)
    print(l)
    
returnes https://github.com/w9w/chaos-hacks/blob/main/download_links.txt

A command

cat download_links.txt | parallel 'wget {} && echo {} | awk "gsub(\"https://chaos-data.projectdiscovery.io/\", \"\")1" | xargs -Irn sh -c "unzip rn && rm rn"'
 
gives a list of all txt files with subdomains
 
 ## How to extract all subdomains into one txt file:
 
 ls | parallel -j 15 "cat {} | tee -a all_hosts.txt >/dev/null"
