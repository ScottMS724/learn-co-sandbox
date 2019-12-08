App Summary: My app idea is a website where users can create to-do lists with tasks/items. Users will be able to mark tasks complete/incomplete, and assign certain tasks to certain users. 

Schema: 
        
        User 
        
        Attributes:    Type:
        Username        String 
        password_digest String
        E-mail          String
        
        
        List 
        
        Attributes:    Type:
        Title            String 
        
        
        Task 
        
        Attributes:    Type:
        Name            String 
        Status          Boolean (default: incomplete)
        list_id         Integer 
        user_id         Integer 
        
        
        
        
Models 

    
    User 
    has_many :tasks 
    has_many :lists, :through => :tasks 
    validates :name, presence: true 
    
    List 
    has_many :tasks 
    has_many :users, :through => :tasks 
    validates :title, presence: true 
    
    Task 
    belongs_to :list 
    belongs_to :user 
    validates :name, presence: true 
  




-"Include a class level ActiveRecord Scope Method" :

            class Task < ActiveRecord::Base
              scope :completed, -> { where(completed: true) }
            end
            
            Task.completed.new.completed    # => true
            Task.completed.create.completed # => true



-"Include signup"


           get '/signup' do
             if !logged_in?
               erb :'users/create_user', locals: {message: "Please sign up before you log in"}
             else
               redirect to '/lists'
             end
           end

-"Include login"

                get '/login' do
                if !logged_in?
                  erb :'users/login'
                else
                  redirect '/lists'
                end
              end

-"Include logout"

                get '/logout' do
                 if logged_in?
                  session.destroy
                  redirect to '/login'
                else
                  redirect to '/'
                end
              end
              
-"Include third party signup/login"

        class SessionsController < ApplicationController
          def create
            @user = User.find_or_create_by(uid: auth['uid']) do |u|
              u.username = auth['info']['username']
              u.email = auth['info']['email']
            end
        
            session[:user_id] = @user.id
        
            render 'welcome/home'
          end

-"Include nested resource show or index"

      <% @user.tasks.each do |task| %>
        <div>
          <h2><%= task.name %></h2>
          <p><%= task.completed? %></p>
        </div>
      <% end %>

-"Include nested resource "new" form"

        <%= form_for(@task) do |f| %>
          <label>Task name:</label><br>
          <%= f.text_field :name %><br>
        
          <label>Task Completed?</label><br>
          <%= f.text_area :completed? %><br>
        
          <%= f.submit %>
        <% end %>

-"Include form display of validation errors"
        