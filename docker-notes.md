


env file

    Commands:

      Specify a env file location:
        # docker-compose --env-file ~/vm/service/.env up -d



## Hash Password

     -  Docker Compoes doesn't like plan text passwords.. so hash it:

        #  echo $(htpasswd -nb <USER> <PASSWORD>) | sed -e s/\\$/\\$\\$/g