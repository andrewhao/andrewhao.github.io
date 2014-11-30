---
layout: post
title: "Explorations in logic programming"
date: 2014-11-29 16:40
comments: true
categories: 
---

Out of a [storybook](https://github.com/andrewhao/storybook), side project I've been doing for a friend, I had the opportunity to model the problem domain as a constraint satisfaction problem (CSP). It goes:

> A set of students are assigned a book a week. Write plan generator to create a set of student-book assignments for that week, given the following constraints:
> 
> * All students must receive a book-bag.
> * Each book-bag may only be assigned to one student at a time.
> * A student may not be assigned a book-bag s/he has received before.

Being that this was a Rails app, I put [Amb](https://github.com/andrewhao/storybook), a Ruby-based CSP solver, to use. Amb is derived off Jim Weirich's original source code, implementing a simple backtracking algorithm.

Not having written any CSP logic since my university days, I tried to come up with a naive solution. It goes something like this:

``` ruby assignment_problem_1.rb https://github.com/andrewhao/storybook/blob/8bb03101d46472e36ee400b79c30d941d3a4bd39/lib/assignment_problem.rb
class AssignmentProblem
  def solve
    # Generates the cross product of sids and bids.
    spaces = student_ids.product(bag_ids)

    # Now generate combinations of those uniques
    full_solution_space = spaces.permutation(student_ids.count).to_a

    # Assign those to the CSP
    plan = solver.choose(*full_solution_space)

    solver.assert all_students_have_bags(plan)
    solver.assert assigned_bags_are_unique(plan)
    solver.assert assigned_bags_without_student_repeats(plan)

    plan
  end
end
```

Well this would work for 2 students and 2 bags, 3 and 3, 4 and 4, but would blow up and overflow the stack when we got to 5 students and 5 bags. Why? I was forcing Ruby to generate the full solution space before starting the CSP search problem:

``` ruby
student_ids = [:s1, :s2]
bag_ids = [:b1, :b2]
# Modeled as: the complete solution space of assignments
spaces = student_ids.product(bag_ids) 
# => [[:s1, :b1], [:s1, :b2], [:s2, :b1], [:s2, :b2]]
```

Then I was taking that cross product and brute-force generating all possible permutations internal to that:

``` ruby
spaces.permutation(student_ids.count).to_a
# => [[[:s1, :b1], [:s1, :b2]], [[:s1, :b1], [:s2, :b1]],
#     [[:s1, :b1], [:s2, :b2]], [[:s1, :b2], [:s1, :b1]],
#     [[:s1, :b2], [:s2, :b1]], [[:s1, :b2], [:s2, :b2]],
#     [[:s2, :b1], [:s1, :b1]], [[:s2, :b1], [:s1, :b2]],
#     [[:s2, :b1], [:s2, :b2]], [[:s2, :b2], [:s1, :b1]],
#     [[:s2, :b2], [:s1, :b2]], [[:s2, :b2], [:s2, :b1]]]
```

The most obvious thing that stood out to me like a sore thumb was how I wasn't simply trusting the constraint-based nature of the problem and expressing the problem in terms of the constraints, instead of attempting to imperatively generate the solution. Usage of `Enumerable#permutation` resulted in an *O(n!)* algorithm, which is unacceptable. Back to the drawing board:

``` ruby assignment_problem_2.rb https://github.com/andrewhao/storybook/blob/e2dd94add2b5949d87968ab650a31b4bdfb9e8a2/lib/csp/assignment_problem.rb
class AssignmentProblem
  # Generates tuples of student => bag assignments
  def solve
    student_ids.each do |sid|
      bid = solver.choose(*bag_ids)
      partial_plan[sid] =  bid
      solver.assert assigned_bags_are_unique(partial_plan)
      solver.assert assigned_bags_without_student_repeats(partial_plan)
    end

    partial_plan.to_a
  end
```

Note the main difference here is how I've re-tooled the solver to assign variables one at a time (in L5) and check constraints after each assignment. This simplifies the domain of the problem as a graph search and helps us more easily reason about this program.

### Further explorations

Follow my [csp-solvers](https://www.github.com/andrewhao/csp-solvers/) project, where I attempt to rewrite this in Prolog and Clojure in an attempt to see how language affects how we reason about problems like these. It's gonna be fun.

### Further reading
* [Artificial Intelligence: A Modern Approach (3rd Edition)](http://www.amazon.com/Artificial-Intelligence-Modern-Approach-3rd/dp/0136042597/ref=sr_1_1?ie=UTF8&qid=1417214576&sr=8-1&keywords=9780136042594)
* http://aima.cs.berkeley.edu/2nd-ed/newchap05.pdf
* [Ruby Forum -- Using Amb](https://www.ruby-forum.com/topic/57768)
* [SchemeWiki: amb](http://community.schemewiki.org/?amb)