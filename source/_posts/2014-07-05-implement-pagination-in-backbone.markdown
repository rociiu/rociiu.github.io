---
layout: post
title: "Implement pagination in backbone"
date: 2014-07-05 17:44:39 +0800
comments: true
categories: 
---

Before today I've written multiple times for a pagination in front end, thought it's good time to write down in blog for future implementation.

## 1. Include pagination in your api for backbone collection

``` ruby

  def index
    customers = current_organization.customers.page(params[:page]).per(25)
    render json: {
      total_count: customers.total_count,
      total_pages: customers.total_pages,
      current_page: customers.current_page,
      per_page: 25,
      models: customers
    }
  end

```

## 2. Implemment paginated backbone collection to handle the api response.

```

  class VcoolineIkcrm.Collections.PaginatedCollection extends Backbone.Collection

    parse: (resp)->
      @page = resp.current_page
      @perPage = resp.per_page
      @totalCount = resp.total_count
      @totalPages = resp.total_pages
      resp.models

    url: ->
      @baseUrl + "?" + $.param({page: @page, perPage: @perPage})

```

## 3. Implement paginated view

``` coffeescript

  class VcoolineIkcrm.Views.Shared.PaginationView extends Backbone.View

    template: JST["backbone/templates/shared/pagination"]

    events:
      "click .page-item": "changePage"

    initialize: (opts={})->
      @collection  = opts.collection
      @currentPage = @collection.page
      @perPage     = @collection.perPage
      @totalCount  = @collection.totalCount
      @totalPages  = @collection.totalPages
      @pageSize    = @collection.length
      @calPageRange()
      @calPages()
      @calBackwardPages()
      @calForwardPages()
      @pageChanged = opts.pageChanged

    changePage: (e)->
      pageNumber = $(e.target).attr('data-page')
      if @pageChanged?
        @pageChanged(pageNumber)

    calPageRange: ->
      @pageStart = (@currentPage - 1) * @perPage + 1
      @pageEnd = ((@currentPage - 1) * @perPage) + @pageSize

    calPages: ->
      min = _.max([1, @currentPage - 2])
      max = _.min([@currentPage + 2, @totalPages])
      @pages = [min..max]

    calBackwardPages: ->
      if _.indexOf(@pages, 1) != -1
        @firstPage = null
      else
        @firstPage = 1
      prevPage = _.max([1, @pages[0] - 1])
      if (_.indexOf(@pages, prevPage) != -1) || prevPage == @firstPage
        @prevPage = null
      else
        @prevPage = prevPage

    calForwardPages: ->
      if _.indexOf(@pages, @totalPages) != -1
        @lastPage = null
      else
        @lastPage = @totalPages
      nextPage = _.min([@totalPages, _.last(@pages) + 1])
      if (_.indexOf(@pages, nextPage) != -1) || nextPage == @lastPage
        @nextPage = null
      else
        @nextPage = nextPage

    render: ->
      if @totalCount == 0
        @$el.html('')
        return
      @$el.html(@template({
        totalCount: @totalCount,
        pageStart: @pageStart,
        pageEnd: @pageEnd,
        pages: @pages,
        currentPage: @currentPage,
        firstPage: @firstPage,
        prevPage: @prevPage,
        lastPage: @lastPage,
        nextPage: @nextPage
      }))
      @

```

## 4. Implement pagination template.

```

  <div class="col-sm-4">
      <small class="text-muted inline m-t-sm m-b-sm">第 <%= pageStart %>-<%= pageEnd %>条 共<%= totalCount %> 条</small>
  </div>
  <div class="col-sm-8 text-right text-center-xs">
      <ul class="pagination pagination-sm m-t-none m-b-none">
          <% if(firstPage != null) { %>
            <li><a class="page-item" data-page="<%= firstPage %>"><i class="fa fa-angle-double-left"></i></a></li>
          <% } %>
          <% if(prevPage != null) { %>
            <li><a class="page-item" data-page="<%= prevPage %>"><i class="fa fa-angle-left"></i></a></li>
          <% } %>
          <% for(var i=0; i < pages.length; i ++) { %>
            <% if(pages[i] == currentPage) { %>
              <li class="active"><a class="page-item" data-page="<%= pages[i] %>"><%= pages[i] %><span class="sr-only">(current)</span></a></li>
            <% } else { %>
              <li><a class="page-item" data-page="<%= pages[i] %>"><%= pages[i] %></a></li>
            <% } %>
          <% } %>
          <% if(nextPage != null) { %>
            <li><a class="page-item" data-page="<%= nextPage %>"><i class="fa fa-angle-right"></i></a></li>
          <% } %>
          <% if(lastPage != null) { %>
            <li><a class="page-item" data-page="<%= lastPage %>"><i class="fa fa-angle-double-right"></i></a></li>
          <% } %>
      </ul>
  </div>

```
