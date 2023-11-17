Answer for Day2 question.

	grep -v "#" wnv[A-G].vcf | cut -f 8 | sed "s/.*DP4=\([0-9]*,[0-9]*,[0-9]*,[0-9]*\);.*/\1/" | sed -E "s/,/\t/g" | awk -F '\t' '{print ($3 + $4)/($1 + $2 + $3 + $4)}'
