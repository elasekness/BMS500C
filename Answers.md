Answer for Day2 question.

	grep -v "#" SRR22764495.vcf | cut -f 8 | sed "s/.*DP4=\([0-9]*,[0-9]*,[0-9]*,[0-9]*\);.*/\1/" | sed -E "s/,/\t/g" | awk -F '\t' '{print ($3 + $4)/($1 + $2 + $3 + $4)}'
