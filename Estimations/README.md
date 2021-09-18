# Estimations

## Types
### Expert Judgement
- asking somebody (usually the expert)
- necessarily imprecise
- it is cheap
- can be off by as much as 100%
- almost always looking for absolute values but these usually start as relative estimations
- only use it when a margin of error of 100% is acceptable
### Corporate Estimation
- relies on multiple sources of estimation
- worthless unless the estimates are blind
- all about the conversation between differing perspectives that none had the complete picture
- when accuracy is the only factor influencing an estimate (no bias, e.g. estimate less calories because you want to consume less) the estimates will vary equally around the correct value (the average/median of all the estimates)
### Analogous Estimation
- also called estimation by historical analogy
- unlike physical/static designs, software design does not work well with historical analogy so a standard deviation is needed (an absolute value with give or take values) but it needs multiple inputs
- basically, find what has happened in the past, which is a good predictor of the future plus or minus a standard deviation
### Parametric Estimation
- plugging values/parameters into a formula
- example of a parameter is the number of providers/clients for a feature
- can be highly accurate when human behaviour is not important to the outcome like automated assembly lines

## Why software estimation is so hard
- software development is research
- unlike physical processes where the steps and the outcome is clear, software development is the application of logic
- by the time you know the steps in SW development the work is done, the term for this in mathematics is ‘a wicked problem’ which must be partially completed before knowing how long it will take

## Recommendations and considerations
- when the difference between 2 estimations is big it’s a sign that the two parties are not talking about the same thing and should be broken down or discussed
- user stories need the who (perspective), what (task), why (value proposition)
- good user stories are also independent, negotiable, valuable, estimable, sized and testable
- prefer relative estimations (Fibonacci style) over absolute (hours/days)
- have a reference story (real and discussed) that is used for relative estimations
- estimation points translate to hours ONLY retrospectively
- use expert judgement and analogous estimation for your own estimations, corporate estimations (planning poker) as a team and parametric estimation using time, velocity, total points estimated graph to estimate project completion which will need introspection
- perform your work - to the degree that it’s possible - in decreasing order of uncertainty and you’ll gain increasing predictability as your project proceeds
- when something can be estimated with high accuracy because it was done multiple times, it is a sign that it can be automated


## Why estimations fail
- user stories with an estimation value not treated as an epic and broken down
- lack of corporate estimations (multiple sources) which reduces the effect of overestimation
- bandwagon effect where an opinion takes over and proper blind estimations do not take place