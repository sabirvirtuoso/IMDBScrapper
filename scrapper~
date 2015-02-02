
titles=($(ls -l movies | grep "^d" | awk '{string="";for(i=9;i<=NF;i++)string=string $i; print string}' | awk -F '[(]|[[]|[.][0-9]{3,}' '{print $1;}' | sed 's/\.//g' | awk -F '[0-9]{3,}[a-zA-z]*' '{print $1;}' | awk -F'(BR)*|(Dv)*|(BD)*' '{print $1;}'));

echo "" > movie-info.txt;

for title in "${titles[@]}"
do
title=$(echo $title | sed -e 's/Trilogy//g' -e 's/Franchise//g' -e 's/Extended[Cut]*//g');
movieId=$(curl "http://www.imdb.com/find?ref_=nv_sr_fn&q="$title | grep -oP "<a href=\"\/title\/.*\/?ref_=fn_al_tt_1\" >.*?<\/a>" | awk '{print $2;}' | awk -F'/' '{print $3;}'); 

content=$(curl "http://www.imdb.com/title/"$movieId"/");

title=$(echo $content | grep -o "<title>.*<\/title>" | sed 's/<.*>\(.*\)<\/.*>/\1/g' | sed -e 's/\&amp;/\&/g' -e 's/\&quot;/\"/g');

moviename=$(echo "$title" | awk -F'(' '{print $1;}');

year=$(echo "$title" | awk -F'(' '{print $2;}' | awk -F')' '{print $1;}');

duration=$(echo $content | grep -o "<time itemprop=\"duration\".*>.*</time>" | sed 's/<.*>\(.*\)<\/.*>/\1/g' );

rating=$(echo $content | grep -oP "<span itemprop=\"ratingValue\">.*?</span>" | sed 's/<.*>\(.*\)<\/.*>/\1/g' );

votes=$(echo $content | grep -oP "<span itemprop=\"ratingCount\">.*?</span>" | sed 's/<.*>\(.*\)<\/.*>/\1/g');

genre=$(echo $content | grep -oP "<span class=\"itemprop\" itemprop=\"genre\">.*?</span>" | sed 's/<.*>\(.*\)<\/.*>/\1/g');

movieInfo="Movie Name: "$moviename"\nYear: "$year"\nDuration: "$duration"\nRating: "$rating"\nTotal Votes: "$votes;

echo -e "$movieInfo" >> movie-info.txt;
echo -e  "Genre: "$genre"\n" >> movie-info.txt;

done



